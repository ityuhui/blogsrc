title: 使用 VSCode 远程运行 Python Jupyter Notebook

date: 2023-10-18 20:57:57
categories:

- Python

tags:

- python

---

## Run Python Jupyter notebook in VSCode on remote host

Steps

1. Install "Remote Development" plugin for VSC
1. Install "Python" plugin for VSC
1. Install jupyter kernel on the remote host

    ```shell
    pip3 install ipykernel
    ```

1. Open the remote directory with VSC,
   Create New Jupyter Notebook command from the Command Palette (Ctrl+Shift+P) or by creating a new .ipynb file in your workspace.

## Jupyter notebooks in Visual Studio Code does not use the active virtual environment

```bash
(venv) $ ipython kernel install --user --name=venv_name
```

Ref: <https://stackoverflow.com/questions/58119823/jupyter-notebooks-in-visual-studio-code-does-not-use-the-active-virtual-environm>
