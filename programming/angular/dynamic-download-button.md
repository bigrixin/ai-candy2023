---
description: 动态下载按钮
---

# ⤵️ Dynamic Download Button

```
// Some code
```

```
// Download state
isDownloading = false;
     

downloadQRCode(): void {
    this.isDownloading = true;

    // 使用 setTimeout 来模拟异步操作，确保 UI 更新
    setTimeout(() => {
         …..
      }

      this.isDownloading = false;
    }, 500);
  }


 <button type="button" class="btn btn-outline-success btn-sm" (click)="download()" title="Download QR Code" [disabled]="isDownloading">
          <i [class]="isDownloading ? 'fa fa-spinner fa-spin' : 'fa fa-download'" aria-hidden="true"></i>
          {{ isDownloading ? 'Downloading...' : 'Download' }}
</button>

```





