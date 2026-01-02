<%*
// 1. 获取当前编辑器中选中的文本
let selection = tp.file.selection();

if (!selection) {
    new Notice("请先选中一段带 aligned 环境的公式代码！");
    return;
}

// 2. 正则表达式处理：移除 \begin{aligned} 和 \end{aligned}
// 同时移除每一行开头的 & (左对齐锚点)
let cleanText = selection
    .replace(/\$\$\s*\\begin\{aligned\}/g, "") // 移除头部的 $$ 和 begin
    .replace(/\\end\{aligned\}\s*\$\$/g, "")   // 移除尾部的 end 和 $$
    .replace(/^&/gm, "")                        // 移除每一行行首的 &
    .trim();

// 3. 将纯净的代码放入剪贴板
await navigator.clipboard.writeText(cleanText);

// 4. 给个提示
new Notice("✨ 已剥离渲染代码，纯净 LaTeX 已存入剪贴板，去 AxMath 粘贴吧！");
%>