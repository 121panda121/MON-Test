```// 自动点击 "get testnet mon" 按钮的脚本
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