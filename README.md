1. 官方网站脚本  
```bash
function autoClickButton() {
    // 查找包含 "get testnet mon" 文本的按钮
    // 注意：这里假设按钮文本是 "Get Testnet MON"，大小写敏感
    const buttons = document.getElementsByTagName('button');
    let targetButton = null;
    
    // 遍历所有按钮，找到目标按钮
    for (let btn of buttons) {
        if (btn.textContent.trim().toLowerCase() === 'get testnet mon') {
            targetButton = btn;
            break;
        }
    }
    
    if (targetButton) {
        // 每隔 1 秒点击一次按钮
        setInterval(() => {
            targetButton.click();
            console.log('按钮已点击');
        }, 10000); // 1000毫秒 = 1秒，可根据需要调整间隔
    } else {
        console.log('未找到 "get testnet mon" 按钮');
    }
}

// 执行脚本
autoClickButton();
```
2. 小幽灵钱包脚本  
```bash
   // 自动点击 "Claim MON" 并根据条件刷新或暂停的脚本
function autoClickAndRefresh() {
    // 查找 "Claim MON" 按钮
    const buttons = document.getElementsByTagName('button');
    let targetButton = null;
    
    for (let btn of buttons) {
        if (btn.textContent.trim().toLowerCase() === 'claim mon') {
            targetButton = btn;
            break;
        }
    }
    
    if (targetButton) {
        console.log('找到 "Claim MON" 按钮，开始自动点击');
        let clickInterval = setInterval(() => {
            targetButton.click();
            console.log('按钮已点击');
            
            // 检查页面是否出现 "Unable to claim at this time"
            if (document.body.textContent.includes('Unable to claim at this time')) {
                console.log('检测到 "Unable to claim at this time"，停止点击，等待 5 分钟...');
                clearInterval(clickInterval); // 停止点击
                setTimeout(() => {
                    console.log('5 分钟等待结束，刷新页面并重启脚本');
                    location.reload(); // 5 分钟后刷新页面
                }, 300000); // 300000 毫秒 = 5 分钟
            }
        }, 1000); // 每 1 秒点击一次
        
        // 正常情况下，每 30 秒刷新一次
        setTimeout(() => {
            console.log('30 秒已到，刷新页面...');
            clearInterval(clickInterval); // 停止当前点击循环
            location.reload(); // 刷新页面
        }, 30000); // 30000 毫秒 = 30 秒
    } else {
        console.log('未找到 "Claim MON" 按钮，可能是页面未加载完成，等待后重试...');
        setTimeout(autoClickAndRefresh, 2000); // 等待 2 秒后重试
    }
}

// 页面加载完成后启动脚本
window.addEventListener('load', () => {
    console.log('页面加载完成，启动脚本');
    autoClickAndRefresh();
});

// 初始运行（如果页面已经加载）
if (document.readyState === 'complete') {
    autoClickAndRefresh();
}
```
