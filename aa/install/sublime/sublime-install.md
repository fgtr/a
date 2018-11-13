### sublime

#### install
```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

> http://www.sublimetext.com/docs/3/linux_repositories.html#apt
> http://blog.csdn.net/coding99/article/details/52421337

#### update
```
#如果下面两条没用，就先执行这个再执行下面两条
sudo dpkg -r sublime-text-installer

sudo apt-get update
sudo apt-get upgrade sublime-text
```
> https://askubuntu.com/questions/828226/how-to-update-sublime-text-3-in-ubuntu-16-04
> 2017-10-15 15:54:52

#### init
+ keys
    * 列编辑
    ```
    { "keys": ["alt+shift+up"], "command": "select_lines", "args": {"forward": false} },
    { "keys": ["alt+shift+down"], "command": "select_lines", "args": {"forward": true} },
    ```
    > http://www.cnblogs.com/Mrsylar/p/6806231.html
    > 2017-07-05 16:41:56
+ package control
    * https://packagecontrol.io/installation

+ Soda Theme
    * pci 
        Package Control: Install Package menu item. The Soda Theme package is listed as Theme - Soda in the packages list.
    * Colour Schemes
        http://buymeasoda.github.com/soda-theme/extras/colour-schemes.zip
    * `Sublime Text 3\Packages\User\Preferences.sublime-settings`
        - 白
            ```
            {
                "color_scheme": 
                    "Packages/User/Espresso Soda.tmTheme",
                    // "Packages/Color Scheme - AA/Mac Classic - AA.tmTheme",
                    // "Packages/Color Scheme - AA/Dawn - AA.tmTheme",
                    // "Packages/User/Espresso Soda.tmTheme",
                "font_face": "Roboto Mono",
                "font_size": 12,
                "ignored_packages":
                [
                    "Markdown",
                    "Vintage"
                ],
                "soda_folder_icons": true,
                "theme": "Soda Light 3.sublime-theme"
            }
            ```
            ubuntu - bak
            ```
            {
                "color_scheme": "Packages/User/Espresso Soda.tmTheme",
                "font_face": "WenQuanYi Zen Hei Sharp",
                "font_size": 10,
                "ignored_packages":
                [
                    "Markdown",
                    "Vintage"
                ],
                "soda_folder_icons": true,
                "theme": "Soda Light 3.sublime-theme"
            }
            ```
        - 黑
            ```
            {
                "color_scheme": "Packages/User/Monokai Soda.tmTheme",
                "font_face": "Arial",
                "font_size": 13,
                "ignored_packages":
                [
                    "Markdown",
                    "Vintage"
                ],
                "soda_folder_icons": true,
                "theme": "Soda Dark 3.sublime-theme"
            }
            ```

+ MarkdownEditing
    `Sublime Text 3\Packages\User\Markdown (Standard).sublime-settings`
    * 白
        ```
        {
            "extensions":
            [
                "md",
                "mdown"
            ],
            "color_scheme": 
            // "Packages/User/Espresso Soda.tmTheme",
            // "Packages/Color Scheme - Default/Sunburst.tmTheme",
            // "Packages/Color Scheme - Default/Dawn.tmTheme",
            // "Packages/Color Scheme - Default/Mac Classic.tmTheme",
            // "Packages/Color Scheme - AA/Mac Classic - AA.tmTheme",
            "Packages/Color Scheme - AA/Dawn - AA.tmTheme",
            "wrap_width": 100,
            // "draw_centered": false,
            // "line_numbers": true,
            "highlight_line": true,
            // Caret
            "caret_style": "solid",   // "wide" is deprecated starting with ST Build 3057.
                                     // In the future, this line will be replaced with:
                                     //     "caret_style": "solid",
            // "caret_extra_width": 1,      
            // These will work only in ST Build 3057 and later.
            "caret_extra_top": 2,
            "caret_extra_bottom": 2
        }
        ```

    * 黑
        ```
        {
            "extensions":
            [
                "md",
                "mdown"
            ],
            "color_scheme": 
            // "Packages/User/Espresso Soda.tmTheme",
            "Packages/Color Scheme - Default/Sunburst.tmTheme",
            // "Packages/Color Scheme - Default/Dawn.tmTheme",
            // "Packages/Color Scheme - Default/Mac Classic.tmTheme",
            // "Packages/Color Scheme - AA/Mac Classic - AA.tmTheme",
            // "Packages/Color Scheme - AA/Dawn - AA.tmTheme",
            "wrap_width": 100,
            // "draw_centered": false,
            // "line_numbers": true,
            "highlight_line": true,
            // Caret
            "caret_style": "solid",   // "wide" is deprecated starting with ST Build 3057.
                                     // In the future, this line will be replaced with:
                                     //     "caret_style": "solid",
            // "caret_extra_width": 1,      
            // These will work only in ST Build 3057 and later.
            "caret_extra_top": 2,
            "caret_extra_bottom": 2
        }
        ```
> https://packagecontrol.io/packages/Theme%20-%20Soda

+  InsertDate
{ "keys": ["f5"], "command": "insert_date", "args": {"format": "%Y-%m-%d %H:%M:%S"}}
