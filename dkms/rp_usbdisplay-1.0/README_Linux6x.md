# rp_usbdisplay 驅動程式 (Linux 6.x 核心支援版)

此版本是針對 Pimoroni `rp_usbdisplay` 驅動程式進行的修改，旨在修復在 Linux 6.x 核心上的編譯與執行問題。

## 主要修改內容

1.  **核心 API 更新**：
    *   修正了 `SetPageReserved` 與 `ClearPageReserved` 的使用方式（在 6.x 核心中已不再需要手動設置 vmalloc 頁面的保留位）。
    *   更新了 `input_dev` 的初始化方式，改用 `input_set_capability` 以符合現代核心規範。
    *   修正了 `fb_info` 的 `flags` 設置，確保與 6.x 核心相容。
2.  **宏定義修復**：
    *   修復了 `err` 與 `info` 宏定義可能導致的衝突，改用核心標準的 `pr_err` 與 `pr_info`。
3.  **標頭檔包含**：
    *   在多個源文件中添加了 `<linux/version.h>` 以支援條件編譯。

## 安裝方法 (DKMS)

1.  將此目錄複製到 `/usr/src/rp_usbdisplay-1.0`：
    ```bash
    sudo cp -r . /usr/src/rp_usbdisplay-1.0
    ```
2.  使用 DKMS 添加、編譯並安裝：
    ```bash
    sudo dkms add -m rp_usbdisplay -v 1.0
    sudo dkms build -m rp_usbdisplay -v 1.0
    sudo dkms install -m rp_usbdisplay -v 1.0
    ```

## 手動編譯

如果您不想使用 DKMS，也可以直接在 `src` 目錄下編譯：
```bash
cd src
make
sudo insmod rp_usbdisplay.ko
```
