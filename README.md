<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Scanner</title>
    <script src="https://unpkg.com/html5-qrcode"></script>
</head>
<body>
    <h1>QRコードリーダー</h1>
    <div id="reader" style="width: 100%; max-width: 500px;"></div>

    <script>
        // ★ここに新しくデプロイした「GASのウェブアプリURL」を貼り付けてください
        const GAS_WEB_APP_URL = "https://script.google.com/macros/s/AKfycbzKEted-9tW00OJbf9syjvVp3h6nHE6t-StAQGxmPD-rEuHffDkLcWoEHpWvZjKXchH8A/exec";

        function onScanSuccess(decodedText, decodedResult) {
            console.log(`Scan result: ${decodedText}`);
            
            // GASへデータを送信
            fetch(GAS_WEB_APP_URL, {
                method: "POST",
                mode: "no-cors", // GASへの簡易リクエスト用
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({ text: decodedText })
            })
            .then(() => {
                alert("データをスプレッドシートに記録しました: " + decodedText);
            })
            .catch(err => {
                console.error("エラーが発生しました:", err);
                alert("送信に失敗しました");
            });
        }

        // カメラの起動
        const html5QrcodeScanner = new Html5QrcodeScanner(
            "reader", { fps: 10, qrbox: 250 }
        );
        html5QrcodeScanner.render(onScanSuccess);
    </script>
</body>
</html>
