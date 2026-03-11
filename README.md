# Chinese Sentence Input Method

A lightweight input method combining a C++ Windows low-level keyboard hook with a Python backend. Keystrokes are captured by the hook and sent via TCP to a Python server, which stores typing history in a local SQLite database. The database is built purely from user input, enabling personalized phrase suggestions without any AI models or pre-trained data. Ideal for privacy-focused input logging or experimenting with custom typing assistance.

---

## 中文简介

本项目实现了一种轻量级输入法，结合C++ Windows底层键盘钩子和Python后端。按键由C++钩子捕获，通过TCP连接发送至Python服务器，服务器将用户输入历史存入本地SQLite数据库。数据库完全由用户自身输入构建，无需AI模型或预训练数据，可实现个性化短语建议。适合隐私优先的输入日志或自定义输入辅助实验。

---

## File Structure / 文件结构

- `cs_copy.py` – Python UI & logic / Python界面与逻辑处理
- `C.exe` – Keyboard hook (C++) / 键盘钩子程序
- `config.json` – Style configuration / 样式配置文件
- `data.db` – SQLite database (auto-generated) / 自动生成的数据库

---

## Quick Start / 快速开始

1. Double-click `C.exe` and `cs_copy.py` (order doesn't matter).  
   双击运行 `C.exe` 和 `cs_copy.py`（不分先后）。
2. The Python server will start and wait for connections.  
   Python服务器启动并等待连接。
3. Start typing – the database will populate automatically.  
   开始打字，数据库会自动构建。

> You may rename the `.exe` files, but `config.json` and `data.db` must keep their original names.  
> 可重命名exe文件，但配置文件和数据库不可改名。

---

## Usage Instructions / 使用说明

### Hotkeys / 热键
| Keys | Function |
|------|----------|
| `Ctrl+Alt+M` | Toggle hook on/off (capture characters) / 启动/关闭钩子（拦截输入） |
| `Ctrl+Alt+Enter` | Toggle hook + UI visibility / 启动/关闭钩子+UI显示 |
| `Ctrl+Alt+P` | Stop hook service / 关闭钩子服务 |

### Typing Interface / 打字界面
- UI displays in **five rows**:  
  界面显示五行：
  1. Current Pinyin input + system split  
     当前拼音及程序拆分
  2. Candidate Pinyin syllables  
     预选拼音
  3. Candidate Chinese characters (single)  
     预选汉字
  4. Pinyin correction row (if error detected)  
     拼音纠错行
  5. Candidate phrases / sentences  
     预选句子

- **Pinyin row symbols**:  
  - `>` `<` indicate the row is selected.  
  - `|||` separates pre-committed characters (left) from pending Pinyin (right).  
  - `|·|` shows cursor position for inserting new characters.

### Basic Operations / 基本操作
| Key | Action |
|-----|--------|
| Arrow keys | Navigate candidates / 方向键选择候选词 |
| Space | Select candidate / 选择当前候选 |
| Enter | Commit pre-committed characters; if none, move first pending Pinyin to pre-commit / 上屏预上屏字符；若无，则将未确认拼音最左侧字符移至预上屏 |
| Backspace | Delete character left of cursor (including pre-committed) / 删除光标左侧字符（包括预上屏） |
| Enter (when no pending Pinyin) | Hide UI, pass through system keys / 隐藏UI，放行系统按键 |

> Tip: Best for typing sentences; for single characters, using the system IME might be faster.  
> 建议：适合长句输入；单字输入可直接使用系统输入法。

---

## Custom Styling / 自定义样式

编辑`config.json` to adjust appearance:

```json
{
  "window": {
    "width": 600,          // UI width / 宽度
    "height": 300,         // UI height / 高度
    "x": 100,              // Initial X position / 初始X坐标
    "y": 100               // Initial Y position / 初始Y坐标
  },
  "grid": {
    "width": 80,           // Candidate button width / 候选按钮宽度
    "height": 30,          // Candidate button height / 候选按钮高度
    "font_size": 14        // Font size / 字体大小
  },
  "colors": {
    "bg": "#F0F0F0",               // Background / 背景色
    "text": "#000000",              // Text / 文字色
    "selected_bg": "#3399FF",       // Selected button background / 选中按钮背景
    "selected_text": "#FFFFFF",     // Selected button text / 选中按钮文字
    "first_row_bg": "#E0E0E0",      // Pinyin row background / 拼音行背景
    "border": "#CCCCCC"             // Border color / 边框色
  },
  "rows": 5,                // Number of rows (fixed for now) / 行数（目前固定）
  "jinum": 10               // Frequency to write to DB (after how many chars) / 每输入多少字符写入数据库一次
}
```

---

## Notes / 注意事项
- Ensure both `C.exe` and `cs_copy.py` are running simultaneously.  
  确保两个程序同时运行。
- Python dependencies: `tkinter`, `sqlite3`, `socket` (all built-in).  
  Python依赖均为内置模块，无需额外安装。
- The database is created automatically on first run.  
  数据库在首次运行时自动创建。

---

## Tech Stack / 技术栈
- **C++**: Windows Hook, Winsock  
- **Python**: Socket, SQLite, Tkinter (UI)

---

## License / 许可证
MIT (or choose your own)
