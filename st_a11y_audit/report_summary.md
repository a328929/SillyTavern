# SillyTavern 1.16.0 前端无障碍静态审计摘要
- 扫描范围：`public/` 与 `default/public/` 下 HTML 模板 + CSS（不含运行时页面状态）
- HTML 类问题：**1147** 条（High 545 / Medium 601 / Low 1）
- CSS 类问题：**23** 条（主要为焦点可见性被移除）

## Top 问题类型（按出现次数）
- Form control missing programmatic label: 559
- Non-semantic element used as button (.menu_button on non-interactive tag): 391
- Interactive icon control missing accessible name: 170
- Link used as button (href="#" / onclick return false): 9
- Image missing alt: 7
- Missing document language: 6
- Disables browser zoom (user-scalable=no / maximum-scale=1): 2
- Possibly non-decorative image has empty alt: 1
- Form control relies on title attribute only: 1
- Dialog missing accessible name: 1

## Top 影响文件（按问题数）
- public/index.html: 843
- public/scripts/extensions/stable-diffusion/settings.html: 42
- public/scripts/extensions/quick-reply/html/settings.html: 26
- public/scripts/extensions/attachments/manager.html: 20
- public/scripts/extensions/regex/dropdown.html: 20
- public/scripts/extensions/quick-reply/html/qrEditor.html: 19
- public/scripts/templates/admin.html: 15
- public/scripts/extensions/vectors/settings.html: 11
- public/login.html: 10
- public/scripts/extensions/memory/settings.html: 9

## 关键结论（摘要）
- 大量控件使用 `<div>/<span>` 作为按钮/图标按钮（依赖 `a11y.js`/`keyboard.js` 补角色与 tabIndex），导致：可访问名称缺失、键盘行为不完整（Space）、焦点顺序不可控。
- 大量表单控件（range/number/select/textarea 等）只有视觉文本（`<small>`/`<span>`）但缺少程序化 label 绑定。
- `meta viewport` 禁止缩放（`user-scalable=no` + `maximum-scale=1`），对低视力用户是硬阻断。
- CSS 中多处 `outline: none`，尤其对滑块句柄等控件会导致“看不见焦点”。
- `dialog` 弹窗缺少可访问名称（`aria-labelledby`），内部按钮/关闭按钮仍是 div，需补齐语义与标签。
