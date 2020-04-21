# Managing Python requirements.txt

When creating a new virtualenv you can use the following to create a requirements.txt file.

```bash
pip freeze > requirements.txt
```

However, this often will include extra packages you may not want included. For example, I often install ipython for developmental 
purposes but do not require it for a program to run. Additionally, I often wind up installing extra packages in a virtualenv to
test things out that I do not want to be included.

To rectify this, you can use [pipreqs](https://github.com/bndr/pipreqs). Install `pipreqs` with:

```bash
pip install pipreqs
```

`pipreqs` will scan your code to determine which modules are required. Then you can generate a requirements file that only includes the necessary modules.

```bash
pipreqs .
# You must use --force to overwrite requirements.txt
pipreqs --force .
```
