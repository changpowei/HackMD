# Docker

###### tags: `docker`

- Docker相關指令：
    - https://blog.longwin.com.tw/2017/01/docker-learn-initial-command-cheat-sheet-2017/
    - https://evshary.github.io/2018/05/12/docker-%E7%B0%A1%E6%98%93%E6%95%99%E5%AD%B8/

## :memo:Step to build container
![](https://i.imgur.com/FUgJMWi.png)
---
### Step 1: 檢查docker的版本
-  `docker -v`
版本需要19.03以上


---
### Step 2: 找環境
- https://ngc.nvidia.com/catalog/all
- 如tensorflow
    環境不是越新越好，要搭配tensorflow的需求
  - https://docs.nvidia.com/deeplearning/frameworks/tensorflow-release-notes/rel_19.10.html#rel_19.10

- 下載
此環境支援tensorflow 2.1.0，cudnn為7.6.4，CUDA為10.1
```
$ sudo docker pull nvcr.io/nvidia/tensorflow:19.10-py3
```

- 查看下載的 image
```
$ sudo docker images
```
![](https://i.imgur.com/HSboq9u.png)

- 刪除image
```
$ sudo docker rmi <image-id>
```
- 確認nvidia
```
$ nvidia-smi
```

- 確認cuda
```
$ nvcc -V
```

---
### Step 3: 執行image 創建 container (run)
透過run將image跑起產生container，同時命名該container，之後exec該container name，即可再次進入container。
- -\-name: 命名container的名字
    :arrow_right:c95cpw
- -it: 會自動執行 docker pull，跑起來自動執行 bash 程式進入此Container
    :arrow_right:nvcr.io/nvidia/tensorflow:19.10-py3
```
$ sudo docker run --gpus all --name c95cpw -it nvcr.io/nvidia/tensorflow:19.10-py3 /bin/bash
```
---
## Docker 相關指令


### Docker 離開 Container (exit)
- 在container內直接輸入exit，即可離開並關閉container。
```
$ exit
```
![](https://i.imgur.com/fi5GZze.png)

---
### Docker 啟動 Image (start)
- c95cpw為創建container時所命名之container name
```
$ sudo docker start c95cpw
```
> **離開container後，要先啟動(start) image一次，之後才能執行(exec) container再進入container。**
> **否則出現以下錯誤：**

---


### Docker 執行 Container 進入 bash (exec)
- 透過進入bash開啟Container
```
$ docker exec -it c95cpw /bin/bash
```
---

### Docker 列出 Container (ps)    
- 執行、停止的 Container 都列出來
```
$ sudo docker ps -a
```
- 還在執行中的 Container，可以看到詳細 hash id
```
$ sudo docker ps
```
---
### Docker 暫停 Container (stop)
- 刪除container前，指定container name 暫停 running的container。
```
$ sudo docker stop <hash-id>
```
---
### Docker 刪除 Container (rm)
- **暫停**的container，指定CONTAINER ID刪除
```
$ sudo docker rm <hash-id>
```
- **未暫停**的container，強制刪除
```
$ sudo docker rm -f <hash-id>
```
---
### Docker 查看 Container 資訊 (inspect)
- 列出container所有訊息
> c95cpw為container name
```
$ sudo docker inspect c95cpw
```
---
### Docker 複製東西進出 Container (cp)

- 一個檔案
    - ~/requirement.txt：為本地端資料夾下之檔案
    - 傳送到container name為c95cpw下的/workspace
    ```
    $ sudo docker cp -a ~/requirement.txt c95cpw:/workspace 
    ```
    可以看到/workspace下有新增requirement.txt檔案。
    ![](https://i.imgur.com/pMpPpFk.png)
- 一整個資料夾
    >傳送方式與單個檔案相同，只是路徑改為整個本地端資料夾。 
    
    ```
    $ sudo docker cp -a ~/Model_visualize c95cpw:/workspace/Model_visualize
    ```
    ![](https://i.imgur.com/Wl31kGe.png)
- 傳出
    > 傳進或傳出只差別在順序前後相反。
    ```
    $ sudo docker cp -a c95cpw:/workspace/Model_visualize ~/Model_visualize
    ```
---
## 使用volume保存容器內的數據
- **何謂volume?**
    簡單來說 Volume 就是用來保存容器內的資料的，看看下面這張圖
    ![](https://i.imgur.com/DBKObof.png)
    當你使用 volume 時，docker 會在你的本機上隨機新增一個資料夾（Local storage area），大部分會在` /var `底下，然後讓這個資料夾跟 container 裡面的某個資料夾互通。
    
    因為他們是互通的，所以**當你 container 裡面那個資料夾有任何變更時，本地的資料夾也會跟著變**，而且很重要的一點是：==**container 被刪掉時那個資料夾還會原封不動保留在那邊**==
- **實際操作**
    - **Docker 產生、操作 Volumes (volume)**
    
         - 建立 local volume (volume是建立在本地端)
        ```
       $ sudo docker volume create --name myvol      #myvol為設定的volume name
        ```
        - Container start 就 Mount 此 volume
        ```
        $ sudo docker run -v myvol:/data
        ```
        -  砍掉 volume
        ```
        $ sudo docker volume rm myvol
        ```
        - 列出 volumes
        ```
        $ sudo docker volume ls
        ```
        ![](https://i.imgur.com/yFliWOc.png)
    - **創建container時 掛載目錄進入 Container (run -v)**
         在啟動時加一個 `-v` 參數，就可以指定 volume 要跟容器內哪一個資料夾連通，這邊用的是 `/db/data`，實際上使用時可以換成資料庫存放資料的路徑。
         
         - 先確認 `/db/data `裡面什麼檔案都沒有
        ```
        $ sudo docker run -v myvol:/db/data -it nvcr.io/nvidia/tensorflow:20.02-tf2-py3 ls -l /db/data
        ```
        ![](https://i.imgur.com/JnsTefv.png)
      - 在container中`/db/data/`新增一個檔案 `file`，會串連到本地端的myvol中
      ```
      $ sudo docker run -v myvol:/db/data -it nvcr.io/nvidia/tensorflow:20.02-tf2-py3 touch /db/data/file
        ```
        
         or 直接***指定某個資料夾***跟容器內的資料夾連通
         ```
         $ sudo docker run -v ~/tensorflow_container:/db/data -it nvcr.io/nvidia/tensorflow:20.02-tf2-py3 ls -l /db/data
         ```
        
      - 故再次確認container中含有`file`的檔案
      ![](https://i.imgur.com/Vj5jrV7.png)
- **查詢volume的資料夾於本地端電腦的位置**
    - 透過inspect查詢，為Mounts下的資訊。
    ```
    $ sudo docker inspect -f '{{.Mounts}}' c95cpw
    ```
---

## 打包並傳送
- 打包成image
> -m : 輸入註解
> -a : 輸入作者
> c95cpw : container name
> c95cpw_test : 打包後的 image name
```
$ sudo docker commit -m="註解" -a="author" c95cpw c95cpw_test
```
- 將image壓縮成tar
> c95cpw_test_tar.tar : 打包的tar檔
> c95cpw_test : 打包後的 image name
```
$ sudo  docker save -o c95cpw_test_tar.tar c95cpw_test
```
- 資料複製傳輸至其他電腦 (**scp**)
> scp 相關指令：https://blog.gtwang.org/linux/linux-scp-command-tutorial-examples/
> 基本指令語法：
```
scp [帳號@來源主機]:來源檔案 [帳號@目的主機]:目的檔案
````
> 如：server的IP位置要再詢問。
```
$ scp c95cpw_test_tar.tar xxxx@172.126.113.2:~/where
```

- 在另一台電腦上還原tar檔成image
> import 進docker，並還原成image (load)
> -i: 放要 import 的檔案名稱
```
$ sudo docker load -i c95cpw_test_tar.tar
```
- 重新執行image創建container
```
$ sudo docker run --runtime=nvidia --network=host --name c95cpw_copy -it c95cpw_test bash
```
## 上傳Docker Hub
- 登入
```
$ sudo docker login
```
- tag 自己的 dockerhub id
```
$ sudo docker tag c95cpw_test ld5874123/c95cpw_test_image
```
- 上傳 docker hub
```
$ sudo docker push ld5874123/c95cpw_test_image
```
- 從 Docker Hub Pull Docker Image 下來
```
$ sudo docker pull ld5874123/c95cpw_test_image
```
- 啟動 Docker Container
```
$ sudo docker run --runtime=nvidia --name=c95cpw -it c95cpw_test bash
```