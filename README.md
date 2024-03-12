# huggingface 下载

# A. 直接下载

通过外国服务器下载到本地，再由本地上传到服务器上

sftp -oPort=   user@ip 

# B. 通过国内镜像网站下载

[HF-Mirror - Huggingface 镜像站](https://hf-mirror.com/)

## B.1. 在镜像网站不用VPN就可以直接下载到本地，然后上传到服务器上，或者使用Wget

## B.2. 使用huggingface-cli工具(hugging face官方提供)

1. 安装依赖: `pip install -U huggingface_hub`
2. 设置环境变量
    
    ```bash
    # 打开配置文件
    vim ~/.bashrc
    
    # 在文件最后输入
    export HF_ENDPOINT=https://hf-mirror.com
    
    # 更新配置
    source ~/.bashrc
    ```
    
3. 下载模型 `huggingface-cli download --resume-download repo_id --local-dir ./`

## ***B.3. 使用hfd脚本（基于git + aria2)（推荐使用这个）***

### B.3.1. 无root安装aria2

1. 下载tar.gz包： `wget https://github.com/aria2/aria2/releases/download/release-1.37.0/aria2-1.37.0.tar.gz`
    
    [aria2](https://aria2.github.io/)
    
2. 解压缩: `tar -zxvf aria2-1.37.0.tar.gz` 
3. 修改配置
    
    ```bash
    cd aria2-1.37.0
    vim configure
    # 找到prefix=xxxx 这一行，这一个是安装目录，需要更换到一个自己有权限写的目录下
    # 例如
    prefix = /home/zhangsan/.local/aria2
    ```
    
4. 安装 
    
    ```bash
    ./configure
    make
    ```
    
5. 配置环境变量
    
    ```bash
    vim ~/.bashrc
    # 在最后一行加 前面更改的prefix加上bin
    export PATH=/home/zhangsan/.local/aria2/bin:$PATH  
    # 更新环境
    source ~/.bashrc
    ```
    

### B.3.2. 下载hfd

[官方的hfd](https://github.com/EuterpeK/hfd-woca/blob/main/hfd.sh)

注意：因为aria2 1.1.0版本之后都会检查CA证书，这一块本人不懂，为了避免麻烦就不认证。但是官方hfd使用aria2时需要认证，因此，对官方的hfd进行更新，添加 `--check-certificate=false` 

1. 下载 hdf-woca: `wget https://raw.githubusercontent.com/EuterpeK/hfd-woca/main/hfd.sh`
2. 更改权限并配置
    
    ```bash
    chmod a+x hfd.sh
    
    # 配置环境变量
    vim ~/.bashrc
    
    # 添加
    export HF_ENDPOINT=https://hf-mirror.com
    
    # 更新环境变量
    source ~/.bashrc
    ```
    
3. 下载模型 `./hfd.sh repo_id --tool aria2c -x 4`
    
    ```bash
    ./hfd.sh repo_id --tool aria2c -x 4
    
    # 解析
    --tool使用的工具：默认是wget
    -x 表示多少线程
    ```
    

# B.4. 使用github的另一个开源工具

[https://github.com/LetheSec/HuggingFace-Download-Accelerator](https://github.com/LetheSec/HuggingFace-Download-Accelerator)

基于hugging_cli的工具

# C. 使用商业工具（低速免费）

[互链高科](https://e.aliendao.cn/#/)

1. 下载脚本 `wget https://[e.aliendao.cn/model_download.py](https://e.aliendao.cn/model_download.py)`
2. 下载
    
    ```bash
    pip install huggingface_hub
    python model_download.py --repo_id repo_id
    ```
