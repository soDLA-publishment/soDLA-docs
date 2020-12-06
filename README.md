# soDLA-docs

sodla-docs.nprocessor.com 

## Development

#### Install Python3.8 & mkdocs

```bash
$ python --version
Python 3.8.2
$ pip --version // or pip3 
pip 20.0.2
```
install mkdocs

```bash
$ pip install mkdocs && pip install --user -r requirements.txt // or pip3
```

#### Edit under Windows      

```bash
$ mkdocs serve
INFO    -  Building documentation...
INFO    -  Cleaning site directory
[I 160402 15:50:43 server:271] Serving on http://127.0.0.1:8000
[I 160402 15:50:43 handlers:58] Start watching changes
[I 160402 15:50:43 handlers:60] Start detecting changes
```

Open up <http://127.0.0.1:8000> in your browser, and you will see the documentation home page being displayed. The dev-server also supports auto-reloading, and will rebuild your documentation whenever anything in the configuration file or the documentation directory changes.
