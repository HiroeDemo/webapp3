# Streamlit 画像処理アプリ

このアプリは、Streamlit を用いて作成された画像処理アプリです。
ユーザーは画像をアップロードし、縮小比率や画質、フィルターを選択して画像を処理することができます。

## 機能

1. 画像アップロード: png, jpg, jpeg 形式をサポート
2. 縮小比率設定: 1~10 までのスライダーで調整可能
3. 画質設定: 1~100 までのスライダーで調整可能
4. フィルター選択: 'None', 'Grayscale', 'Sepia' の3種類から選択可能

## 使い方

1. "画像をアップロードしてください。" から画像を選択。
2. "縮小比率 (1/?)" スライダーで縮小比率を設定。
3. "画質 (1-100)" スライダーで画質を設定。
4. "フィルターを選択" でフィルターを選択。

## コード例

```python
import streamlit as st
from PIL import Image, ImageOps
import numpy as np
import io

# 画像アップロード
uploaded_image = st.file_uploader("画像をアップロードしてください。", type=["png", "jpg", "jpeg"])

if uploaded_image is not None:
    image = Image.open(uploaded_image)

    # 画像表示（アップロード前）
    st.image(image, caption="Uploaded Image", use_column_width=True)

    # スライダーで設定
    ratio = st.slider("縮小比率 (1/?)", 1, 10, 2)
    quality = st.slider("画質 (1-100)", 1, 100, 85)

    # フィルター選択
    filter_option = st.selectbox("フィルターを選択", ["None", "Grayscale", "Sepia"])

    # 画像処理
    new_size = (image.width // ratio, image.height // ratio)
    image = image.resize(new_size, Image.ANTIALIAS)

    if filter_option == "Grayscale":
        image = ImageOps.grayscale(image)
    elif filter_option == "Sepia":
        np_image = np.array(image)
        sepia_filter = np.array([[0.393, 0.769, 0.189],
                                 [0.349, 0.686, 0.168],
                                 [0.272, 0.534, 0.131]])
        sepia_image = np_image @ sepia_filter.T
        sepia_image = np.clip(sepia_image, 0, 255)
        image = Image.fromarray(sepia_image.astype('uint8'), 'RGB')

    # 画質調整と表示
    img_byte_arr = io.BytesIO()
    image.save(img_byte_arr, format="JPEG", quality=quality)
    img_byte_arr = img_byte_arr.getvalue()
    
    st.image(img_byte_arr, caption="Processed Image", use_column_width=False, channels="RGB", output_format="JPEG")
```
