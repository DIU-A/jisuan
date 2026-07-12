<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>服务器部署测试页面</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft Yahei", sans-serif;
        }
        body {
            background-color: #f5f7fa;
            padding: 50px 20px;
        }
        .container {
            max-width: 700px;
            margin: 0 auto;
            background: #fff;
            padding: 40px;
            border-radius: 12px;
            box-shadow: 0 2px 15px rgba(0,0,0,0.08);
        }
        h1 {
            color: #2563eb;
            text-align: center;
            margin-bottom: 30px;
        }
        .info-item {
            padding: 16px;
            border-bottom: 1px solid #eee;
            font-size: 16px;
        }
        .label {
            font-weight: bold;
            color: #333;
            display: inline-block;
            width: 140px;
        }
        .success-tip {
            margin-top: 30px;
            padding: 20px;
            background-color: #dcfce7;
            color: #166534;
            text-align: center;
            font-size: 18px;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>✅ 域名/服务器部署测试成功</h1>

        <div class="info-item">
            <span class="label">当前访问域名：</span>
            <span id="domain"></span>
        </div>
        <div class="info-item">
            <span class="label">页面访问协议：</span>
            <span id="protocol"></span>
        </div>
        <div class="info-item">
            <span class="label">客户端当前时间：</span>
            <span id="nowTime"></span>
        </div>
        <div class="info-item">
            <span class="label">页面编码：</span>
            UTF-8（中文正常显示）
        </div>
        <div class="info-item">
            <span class="label">测试状态：</span>
            HTML静态文件可正常加载
        </div>

        <div class="success-tip">
            恭喜！你的服务器域名部署运行正常！
        </div>
    </div>

    <script>
        // 自动填充域名、协议、时间
        document.getElementById('domain').innerText = window.location.host;
        document.getElementById('protocol').innerText = window.location.protocol;
        
        function updateTime() {
            const now = new Date();
            document.getElementById('nowTime').innerText = now.toLocaleString('zh-CN');
        }
        updateTime();
        setInterval(updateTime, 1000);
    </script>
</body>
</html>
