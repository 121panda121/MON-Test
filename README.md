1. 官方网站脚本  
```bash
function autoClickButton() {
    // 查找包含 "get testnet mon" 文本的按钮（不区分大小写）
    const buttons = document.getElementsByTagName('button');
    let targetButton = null;

    for (let btn of buttons) {
        if (btn.textContent.trim().toLowerCase() === 'get testnet mon') {
            targetButton = btn;
            break;
        }
    }

    if (targetButton) {
        console.log('找到目标按钮，开始自动点击');
        // 每隔 10 秒点击一次按钮
        setInterval(() => {
            if (!targetButton.disabled && document.body.contains(targetButton)) {
                targetButton.click();
                console.log('按钮已点击，时间: ' + new Date().toLocaleTimeString());
            } else {
                console.log('按钮不可用或已从页面移除');
            }
        }, 10000); // 10000 毫秒 = 10 秒
    } else {
        console.log('未找到 "get testnet mon" 按钮');
    }
}

// 确保 DOM 加载完成后再执行
if (document.readyState === 'complete' || document.readyState === 'interactive') {
    autoClickButton();
} else {
    document.addEventListener('DOMContentLoaded', autoClickButton);
}
```
2. 小幽灵钱包脚本  
```bash
   // 自动点击脚本，不刷新页面
function autoClick() {
    // 查找 "Claim MON" 和 "Close" 按钮
    function findButtons() {
        const buttons = document.getElementsByTagName('button');
        let claimButton = null;
        let closeButton = null;

        for (let btn of buttons) {
            const text = btn.textContent.trim().toLowerCase();
            if (text === 'claim mon') {
                claimButton = btn;
            } else if (text === 'close') {
                closeButton = btn;
            }
            if (claimButton && closeButton) break; // 找到两个按钮后退出循环
        }
        return { claimButton, closeButton };
    }

    // 主循环逻辑
    function startClicking() {
        let clickInterval = setInterval(() => {
            const { claimButton, closeButton } = findButtons();

            if (!claimButton) {
                console.log('未找到 "Claim MON" 按钮，等待 2 秒后重试...');
                clearInterval(clickInterval);
                setTimeout(startClicking, 5000); // 2 秒后重试
                return;
            }

            if (closeButton) {
                console.log('找到 "Close" 按钮，点击关闭...');
                closeButton.click();
            } else {
                console.log('未找到 "Close" 按钮，继续点击 "Claim MON"...');
                claimButton.click();
            }
        }, 10000); // 每 1 秒检查并点击一次
    }

    console.log('启动脚本');
    startClicking();
}

// 页面加载时启动脚本
window.addEventListener('load', () => {
    console.log('页面加载完成，启动脚本');
    autoClick();
});

// 初始运行（如果页面已加载）
if (document.readyState === 'complete') {
    console.log('页面已加载，立即启动脚本');
    autoClick();
}

// 监听 DOM 变化，确保动态加载的按钮也能被检测
const observer = new MutationObserver(() => {
    console.log('检测到 DOM 变化，检查并启动脚本');
    autoClick();
});
observer.observe(document.body, { childList: true, subtree: true });
```
                                                                                                @Twitter：longyueting
