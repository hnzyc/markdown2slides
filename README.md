# markdown2slides
Tools for converting Markdown Source File to Reveal.js HTML5 slides

## Usage:

1. Install Anaconda 3
2. Install Pandoc
3. Clone this github repo
4. cd into the directory markdown2slides
5. Open config.json, and change the information according to your situation
6. run python3 md2slide.py example/myslide.md

## 我遇到的问题

1. 路径有中文字符的问题

    ```python
    (base) PS D:\文档\markdown2slides-master> python .\md2slide.py .\example\myslide.md
    Traceback (most recent call last):
      File ".\md2slide.py", line 16, in <module>
        converter = MarkdownRevealjsConverter(md_fname, **config)
      File "D:\文档\markdown2slides-master\revealjs_converter.py", line 11, in __init__
        super().__init__(*args, **kwargs)
      File "D:\文档\markdown2slides-master\converter.py", line 24, in __init__
        self.config = json.load(f)
      File "C:\Users\zhaoy\Anaconda3\lib\json\__init__.py", line 293, in load
        return loads(fp.read(),
    UnicodeDecodeError: 'gbk' codec can't decode byte 0x89 in position 113: illegal multibyte sequence
    ```

    这是由于编码问题`encoding = utf-8`

    >line 23 of converter.py

    ```
    -      with open(config_json_fname) as f:
    +      with open(config_json_fname, encoding = 'utf-8') as f:
                self.config = json.load(f)
    ```

该问题解决。

2. 系统找不到文件的问题

    ```python
    (base) PS D:\文档\markdown2slides-master> python .\md2slide.py .\example\myslide.md
    系统找不到指定的路径。
    Traceback (most recent call last):
      File ".\md2slide.py", line 17, in <module>
        converter.convert()
      File "D:\文档\markdown2slides-master\revealjs_converter.py", line 25, in convert
        self.pandoc_slide_md_to_revealjs()
      File "D:\文档\markdown2slides-master\revealjs_converter.py", line 59, in pandoc_slide_md_to_revealjs
        with open(self.temp_html) as f:
    FileNotFoundError: [Errno 2] No such file or directory: 'D:\\文档\\markdown2slides-master\\example\\temp.html'
    ```

    看问题描述，`revealjs_converter.py`找不到pandoc的命令，所以line 50修改为

    ```git
    -      cmd = f"""/usr/local/bin/pandoc -t revealjs \
    +      cmd = f"""pandoc -t revealjs \
    ```

问题解决。

3. open不是命令的问题

    ```python
    (base) PS D:\文档\markdown2slides-master> python .\md2slide.py .\example\myslide.md
    'open' 不是内部或外部命令，也不是可运行的程序或批处理文件。
    ```

    这是因为Windows系统里不是`open`而是`start`,所以，修改`revealjs_converter.py`line34

    ```python
    -cmd = f"open {self.output_html}"
    +cmd = f"start {self.output_html}"
    ```

    以上就是我遇到的所有问题，逐一修改之后，问题全部得以解决，可以看到实际的slide了，不过不知道为什么，打开的速度居然那么慢。

    以上问题解决，参考了王树义老师repo的issue以及另外一位的[repo](https://github.com/dumushui/markdown2slides).