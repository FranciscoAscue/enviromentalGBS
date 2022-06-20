#### config tunneling to work remote with jupyter notebook 

### before install kernel in remote server 
### bash kernel
```python3
pip3 install bash_kernel
```

### Install R
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
```

```output
Executing: /tmp/apt-key-gpghome.2KA46cJU5d/gpg.1.sh --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
gpg: clave 51716619E084DAB9: clave pública "Michael Rutter <marutter@gmail.com>" importada
gpg: Cantidad total procesada: 1
gpg:               importadas: 1
```

#### add deb repository for cran-40 and install r-base
```bash
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/'
sudo apt update
sudo apt install r-base
```

#### install r kernel for jupyter notebook
```r
install.packages(c('repr', 'IRdisplay', 'IRkernel'), type = 'source')
IRkernel::installspec(user = FALSE)
```

### Remote access by tunneling ssh to server 

```bash
ssh -N -f -L localhost:8889:localhost:8889 remote_user@remote_host -p port
```
### acces to server and start jupyter notebook

```bash
jupyter notebook --no-browser --port=8889
```

To access to notebook open the local browser on `http://localhost:8889/`



