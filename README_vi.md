# 1. Quick Start:

Xem ví dụ xác thực, gửi và nhận tin nhắn ở đây: https://github.com/azstack/JavaScript-SDK/blob/master/Example/example_1.html


```javascript
<!-- optional -->
<script type="text/javascript" src="js/jquery-1.12.1.min.js"></script>

<!-- required libraries -->
<script type="text/javascript" src="js/socket.io.js"></script>
<script type="text/javascript" src="js/jsencrypt.js"></script>
<script type="text/javascript" src="js/azstack_sdk-1.0-build-20160305-2.js"></script>

<script type="text/javascript">
	var currentMsgId = Math.floor(Date.now() / 1000);

	$(document).ready(function () {
		//init SDK
		var azAppID = "26870527d2ac628002dda81be54217cf";
		var publicKey = 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq9s407QkMiZkXF0juCGjti6iWUDzqEmP+Urs3+g2zOf+rbIAZVZItS5a4BZlv3Dux3Xnmhrz240OZMBO1cNcpoEQNij1duZlpJY8BJiptlrj3C+K/PSp0ijllnckwvYYpApm3RxC8ITvpmY3IZTrRKloC/XoRe39p68ARtxXKKW5I/YYxFucY91b6AEOUNaqMFEdLzpO/Dgccaxoc+N1SMfZOKue7aH0ZQIksLN7OQGVoiuf9wR2iSz3+FA+mMzRIP+lDxI4JE42Vvn1sYmMCY1GkkWUSzdQsfgnAIvnbepM2E4/95yMdRPP/k2Qdq9ja/mwEMTfA0yPUZ7LiywoZwIDAQAB';
		var azStackUserId = 'user2';
		var fullname = 'Dau Ngoc Huy';
		var userCredentials = '274f011805dad952bf234f1cac990f18';
		//log level
		azstack.logLevel = 'INFO';

		//SDK delegate --------------------------------------------- -->
		azstack.onAuthenticationCompleted = function (code, authenticatedUser) {
			azstack.log('INFO', 'onAuthenticationCompleted code ' + code + ', authenticatedUser: ');
			azstack.log('INFO', authenticatedUser);

			$msgDiv.append('onAuthenticationCompleted code ' + code + ', authenticatedUser: ' + '<br />');
			$msgDiv.append(JSON.stringify(authenticatedUser) + '<br />');
		}
		azstack.onMessageReceived = function (user, msg) {
			azstack.log('INFO', 'onMessageReceived');
			azstack.log('INFO', user);
			azstack.log('INFO', msg);
		}
		azstack.onMessagesDelivered = function (packet) {
			azstack.log('INFO', 'onMessagesDelivered');
			azstack.log('INFO', packet);
		}
		azstack.onMessagesSent = function (packet) {
			azstack.log('INFO', 'onMessagesSent');
			azstack.log('INFO', packet);
		}
		azstack.onMessageFromMe = function (packet) {//msg from me (from other device)
			azstack.log('INFO', 'onMessageFromMe');
			azstack.log('INFO', packet);
		}
		//SDK delegate --------------------------------------------- <--

		//start authentication
		azstack.connect(azAppID, publicKey, azStackUserId, userCredentials, fullname);//connect AZStack server
	});
</script>
```

# 2. Tạo ứng dụng và cấu hình:
>	a. Tạo ứng dụng (dự án) của bạn tại đây: https://developers.azstack.co/project/add

>	b. Sau khi tạo ứng dụng thành công, vào menu "Keys" của ứng dụng của bạn, nhấn "Generate a new keys" để lấy Public key, Private key và Secret code. Private key và Secret code sẽ được lưu trên server của bạn để thực hiện việc xác thực. Mô hình xác thực 3 bên giữa Client (web app) của bạn - AZStack server - server của bạn được mô tả ở đây: 
https://github.com/azstack/iOS-SDK-Documentation#32-authentication

>	c. Code mẫu xác thực cho server của bạn viết bằng PHP ở đây: https://github.com/azstack/Backend-example/tree/master/php
	Cấu hình URL thực hiện phần xác thực trên server của bạn ở menu "Authen URL" của ứng dụng của bạn trên https://developers.azstack.co

# 3. Sử dụng JavaScript SDK:
Các thư viện bắt buộc

> a. socket.io: https://github.com/socketio/socket.io-client

> b. jsencrypt: https://github.com/travist/jsencrypt

> c. AZStack SDK

Thư viện tuỳ chọn:
>  JQuery: https://jquery.com/download/


Code mẫu:
```javascript
		<script type="text/javascript" src="js/socket.io.js"></script>
		<script type="text/javascript" src="js/jsencrypt.js"></script>
		<script type="text/javascript" src="js/azstack_sdk-1.0-build-20160305-2.js"></script>
```
# 4. Các delegate

Thực hiện các hàm delegate bắt buộc để nhận sự kiện xác thực thành công, có tin nhắn đến, gửi tin nhắn thành công:
```javascript
//hàm được gọi khi quá trình xác thực hoàn tất
azstack.onAuthenticationCompleted = function (code, authenticatedUser) {
}

//hàm được gọi khi có tin nhắn đến
azstack.onMessageReceived = function (user, msg) {
}

//hàm được gọi khi tin gửi đã đến người nhận
azstack.onMessagesDelivered = function (packet) {
}

//hàm được gọi khi gửi tin nhắn thành công
azstack.onMessagesSent = function (packet) {
}

//hàm được gọi khi có 1 tin nhắn được gửi từ chính tài khoản của mình trên 1 device khác
azstack.onMessageFromMe = function (packet) {
}
```
# 5. Kết nối:
Gọi hàm sau để kết nối đến AZStack server và thực hiện quá trình xác thực:
```javascript
azstack.connect(azAppID, publicKey, azStackUserId, userCredentials, fullname);
```
Trong đó:

> azAppID: ID của ứng dụng của bạn, lấy ở menu "Keys" của ứng dụng của bạn trên https://developers.azstack.co/

> publicKey: RSA public key của ứng dụng của bạn, lấy ở menu "Keys" của ứng dụng của bạn trên https://developers.azstack.co/

> azStackUserId: ID duy nhất (string) với mỗi ứng dụng trên AZStack, xác định duy nhất 1 user trên ứng dụng đó. ID có thể là số điện thoại, email, username, ...

> userCredentials: có thể là mật khẩu hoặc token, ... Thông tin này sẽ được AZStack forward đến server của bạn dùng để xác thực.

> fullname: Tên (được dùng để send push notification đến mobile khi người nhận không online)


