
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第三方网络存储高速下载工具</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        header {
            padding: 40px 20px;
            text-align: center;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 700;
            letter-spacing: 1px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }

        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
            font-weight: 300;
        }

        .disk-selection {
            padding: 30px 20px;
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        .disk-option {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        
        .disk-option:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .disk-option.selected {
            border-color: #6a11cb;
            background-color: #f8f0ff;
        }

        .disk-icon {
            width: 60px;
            height: 60px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            color: white;
            font-size: 1.5rem;
            font-weight: bold;
        }

        .quark-icon { background: #2196F3; }
        .baidu-icon { background: #FF5722; }
        .xunlei-icon { background: #FFC107; }
        .huang-icon { background: #4CAF50; }

        .disk-name {
            font-weight: 600;
            font-size: 1.1rem;
        }

        .form-section {
            padding: 0 30px 30px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }

        input[type="text"], input[type="password"] {
            width: 100%;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            transition: border 0.3s ease;
        }

        input[type="text"]:focus, input[type="password"]:focus {
            border-color: #6a11cb;
            outline: none;
            box-shadow: 0 0 0 2px rgba(106, 17, 203, 0.2);
        }

        .optional {
            font-size: 0.8rem;
            color: #888;
            margin-left: 5px;
        }

        .status-message {
            padding: 10px 15px;
            margin-top: 10px;
            border-radius: 8px;
            background-color: #f0f7ff;
            border-left: 4px solid #2196F3;
            font-size: 0.9rem;
            display: none;
        }

        .btn {
            display: block;
            width: 100%;
            padding: 15px;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 20px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .btn:active {
            transform: translateY(1px);
        }

        .progress-container {
            margin-top: 30px;
            display: none;
        }

        .progress-bar {
            height: 8px;
            background-color: #e0e0e0;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            width: 0;
            background: linear-gradient(90deg, #6a11cb 0%, #2575fc 100%);
            transition: width 0.3s ease;
        }

        .progress-text {
            text-align: center;
            margin-top: 10px;
            font-size: 0.9rem;
            color: #555;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            max-width: 400px;
            width: 90%;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            animation: modal-in 0.3s ease;
        }

        @keyframes modal-in {
            from { opacity: 0; transform: translateY(-50px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .modal-icon {
            font-size: 3rem;
            margin-bottom: 15px;
            color: #FF5722;
        }

        .modal-title {
            font-size: 1.5rem;
            margin-bottom: 10px;
            font-weight: 600;
        }

        .modal-message {
            margin-bottom: 20px;
            color: #555;
            line-height: 1.5;
        }

        .modal-btn {
            padding: 10px 20px;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
        }

        footer {
            text-align: center;
            padding: 20px;
            color: #888;
            font-size: 0.9rem;
        }

        @media (max-width: 600px) {
            h1 {
                font-size: 2rem;
            }
            
            .disk-selection {
                grid-template-columns: repeat(1, 1fr);
            }
            
            .form-section {
                padding: 0 20px 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>第三方网络存储高速下载工具</h1>
            <p class="subtitle">支持多种网盘的高速解析与下载</p>
        </header>

        <div class="disk-selection">
            <div class="disk-option" data-disk="quark">
                <div class="disk-icon quark-icon">夸</div>
                <div class="disk-name">夸克网盘</div>
            </div>
            <div class="disk-option" data-disk="baidu">
                <div class="disk-icon baidu-icon">百</div>
                <div class="disk-name">百度网盘</div>
            </div>
            <div class="disk-option" data-disk="xunlei">
                <div class="disk-icon xunlei-icon">雷</div>
                <div class="disk-name">迅雷云盘</div>
            </div>
            <div class="disk-option" data-disk="huang">
                <div class="disk-icon huang-icon">H</div>
                <div class="disk-name">huang111</div>
            </div>
        </div>

        <div class="form-section">
            <div class="form-group">
                <label for="share-link">分享链接</label>
                <input type="text" id="share-link" placeholder="请输入网盘分享链接">
                <div class="status-message" id="link-status">用户选择的云盘解析服务已就位，如果出现意外，请联系QQ 12345678</div>
            </div>

            <div class="form-group">
                <label for="share-password">分享密码 <span class="optional">(可选)</span></label>
                <input type="text" id="share-password" placeholder="请输入分享密码">
            </div>

            <button class="btn" id="analyze-btn">开始解析</button>

            <div class="progress-container" id="progress-container">
                <div class="progress-bar">
                    <div class="progress" id="progress-bar"></div>
                </div>
                <div class="progress-text" id="progress-text">0%</div>
            </div>
        </div>
    </div>

    <div class="modal" id="error-modal">
        <div class="modal-content">
            <div class="modal-icon">⚠️</div>
            <div class="modal-title" id="modal-title">解析错误</div>
            <div class="modal-message" id="modal-message">解析过程中发生错误，请稍后再试</div>
            <button class="modal-btn" id="modal-close">确定</button>
        </div>
    </div>

    <footer>
        <p>第三方网络存储高速下载工具 &copy; 2025 版权所有</p>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const diskOptions = document.querySelectorAll('.disk-option');
            const shareLinkInput = document.getElementById('share-link');
            const sharePasswordInput = document.getElementById('share-password');
            const analyzeBtn = document.getElementById('analyze-btn');
            const progressContainer = document.getElementById('progress-container');
            const progressBar = document.getElementById('progress-bar');
            const progressText = document.getElementById('progress-text');
            const errorModal = document.getElementById('error-modal');
            const modalTitle = document.getElementById('modal-title');
            const modalMessage = document.getElementById('modal-message');
            const modalClose = document.getElementById('modal-close');
            const linkStatus = document.getElementById('link-status');
            
            let selectedDisk = null;

            // 选择网盘
            diskOptions.forEach(option => {
                option.addEventListener('click', function() {
                    diskOptions.forEach(opt => opt.classList.remove('selected'));
                    this.classList.add('selected');
                    selectedDisk = this.getAttribute('data-disk');
                    
                    // 显示状态消息
                    if (shareLinkInput.value.trim() !== '') {
                        linkStatus.style.display = 'block';
                    }
                });
            });

            // 输入分享链接时显示状态消息
            shareLinkInput.addEventListener('input', function() {
                if (this.value.trim() !== '' && selectedDisk) {
                    linkStatus.style.display = 'block';
                } else {
                    linkStatus.style.display = 'none';
                }
            });

            // 开始解析
            analyzeBtn.addEventListener('click', function() {
                if (!selectedDisk) {
                    showError('请选择网盘类型');
                    return;
                }

                if (shareLinkInput.value.trim() === '') {
                    showError('请输入分享链接');
                    return;
                }

                // 显示进度条
                progressContainer.style.display = 'block';
                progressBar.style.width = '0%';
                progressText.textContent = '0%';
                
                let progress = 0;
                const interval = setInterval(() => {
                    progress += 30;
                    if (progress <= 100) {
                        progressBar.style.width = progress + '%';
                        progressText.textContent = progress + '%';
                    } else {
                        clearInterval(interval);
                        
                        // 显示错误提示
                        let errorMessage = '';
                        switch(selectedDisk) {
                            case 'quark':
                                errorMessage = '夸克云盘接口无响应';
                                break;
                            case 'baidu':
                                errorMessage = '百度云盘接口无响应';
                                break;
                            case 'xunlei':
                                errorMessage = '迅雷云盘接口无响应';
                                break;
                            case 'huang':
                                errorMessage = 'huang111接口无响应';
                                break;
                        }
                        
                        setTimeout(() => {
                            showError('解析失败', errorMessage);
                            progressContainer.style.display = 'none';
                        }, 500);
                    }
                }, 1000);
            });

            // 关闭错误提示
            modalClose.addEventListener('click', function() {
                errorModal.style.display = 'none';
            });

            // 显示错误提示
            function showError(title, message) {
                modalTitle.textContent = title || '解析错误';
                modalMessage.textContent = message || '解析过程中发生错误，请稍后再试';
                errorModal.style.display = 'flex';
            }
        });
    </script>
</body>
</html>
