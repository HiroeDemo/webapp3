## Streamlit Image Resizer

このStreamlitアプリは、アップロードした画像のサイズと画質を調整します。

### 使い方

1. アプリを起動します。
2. 画像をアップロードします。
3. スライダーで縮小比率と画質を調整します。
4. 処理された画像が表示されます。

### ソースコード

```python
import streamlit as st
from PIL import Image
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

    # 画像処理
    new_size = (image.width // ratio, image.height // ratio)
    image = image.resize(new_size, Image.ANTIALIAS)

    # 画質調整と表示
    img_byte_arr = io.BytesIO()
    image.save(img_byte_arr, format="JPEG", quality=quality)
    img_byte_arr = img_byte_arr.getvalue()
    
    st.image(img_byte_arr, caption="Processed Image", use_column_width=False, channels="RGB", output_format="JPEG")
```
