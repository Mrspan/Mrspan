# Generate images and download

>插件
>生成图片插件： html2canvas
>插件


## 生成图片

``` bash
        #获取dom元素
        let ele = document.getElementById('qrcode');
          if (!ele) {
            return;
          }
        let cloneEle = ele.cloneNode(true);
        #方便设置样式可以不设置
        cloneEle.className = 'cloneEle'; 
        cloneEle.style.width = ele.clientWidth + 'px';
        cloneEle.style.height = ele.clientHeight + 'px';
        cloneEle.style.position = 'fixed';
        cloneEle.style.top = '-' + (ele.clientHeight + 50) + 'px';
        cloneEle.style.left = '0';
        document.body.appendChild(cloneEle);
        #vue 语法，可以换做定时器
        this.$nextTick(() => { 
          document.documentElement.scrollLeft = 0;
          html2canvas(cloneEle, {
            width: cloneEle.scrollWidth,
            height: cloneEle.scrollHeight,
            scrollX: 0,
            scrollY: 0,
            scale: 5,
            useCORS: true
          }).then((res) => {
            this.$set(this, 'src', res.toDataURL('image/png'));
            document.body.removeChild(cloneEle);
          });
        });
```

## 下载图片


```bash
        let that = this;
        //兼容ie
        if (window.navigator.msSaveOrOpenBlob) { 
          let str = atob(that.src.split(',')[1]);
          let len = str.length;
          let u8arr = new Uint8Array(len);
          while (len--) {
            u8arr[len] = str.charCodeAt(len);
          }
          let blob = new Blob([u8arr]);
          window.navigator.msSaveOrOpenBlob(blob, 'qrcode.png');
        } else {
          const image = new Image();
          image.setAttribute('crossOrigin', 'anonymous');
          image.onload = function() {
            const a = document.createElement('a');
            const event = new MouseEvent('click');
            a.download = 'qrcode';
            a.href = that.src;
            a.dispatchEvent(event);
          };
          image.src = that.src;
        }
```
