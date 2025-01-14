# Xây Dựng Chatbot AI với LangChain và Python

## Yêu cầu hệ thống

- Python 3.8 trở lên,version 3.8.18 
- Docker Desktop 
- OpenAI API key 

## Các bước cài đặt và chạy

### Bước 1: Cài đặt môi trường

- Dùng python version 3.8.18.
- Nên dùng conda, setup environment qua câu lệnh: conda create -n myenv python=3.8.18
- Sau đó active enviroment qua câu lệnh: conda activate myenv
- Mở Terminal/Command Prompt và chạy lệnh sau:
  - pip install -r requirements.txt

### Bước 2: Tải xuống Ollama

- Truy cập: https://ollama.com/download
- Chọn phiên bản phù hợp với hệ điều hành
- Cài đặt theo hướng dẫn
- Chạy lệnh: ollama run llama2

### Bước 3: Cài đặt và chạy Milvus Database

1. Khởi động Docker Desktop
2. Mở Terminal/Command Prompt, chạy lệnh:
   docker compose up --build

Option: Cài đặt attu để view data đã seed vào Milvus:

1. Chạy lệnh: docker run -p 8000:3000 -e MILVUS_URL={milvus server IP}:19530 zilliz/attu:v2.4
2. 2 Thay "milvus server IP" bằng IP internet local, cách lấy IP local:
   - Chạy lệnh: ipconfig hoặc tương tự với các hệ điều hành khác

### Bước 4: Cấu hình OpenAI API

1. Tạo file `.env`
2. Truy cập OpenAI để lấy OPENAI_API_KEY:https://platform.openai.com/api-keys
3. Thêm API key vào file .env:
  - OPENAI_API_KEY=sk-your-api-key-here

Options: Cấu hình Langsmith:
1. Truy cập langsmith để lấy LANGCHAIN_API_KEY: https://smith.langchain.com/
2. Thêm 4 dòng sau vào .env:
  - LANGCHAIN_TRACING_V2=true
  - LANGCHAIN_ENDPOINT="https://api.smith.langchain.com"
  - LANGCHAIN_API_KEY="your-langchain-api-key-here"
  - LANGCHAIN_PROJECT="project-name"

### Bước 5: Chạy ứng dụng

1. Crawl data về local 
Mở Terminal/Command Prompt, di chuyển vào thư mục src  `cd src` và chạy:
```python
python crawl.py
```
2. Seed data vào Milvus:
```python 
python seed_data.py
```
(Kiểm tra data đã aào Milvus chưa bằng cách truy cập: http://localhost:8000/#/databases/default/colletions
<Nhớ để ý `docker run -p 8000:3000 -e MILVUS_URL={milvus server IP}:19530 zilliz/attu:v2.4` để chắc chắn Milvus đang hoạt động >)

3. Run ứng dụng:
```python
streamlit run main.py
```

## Cách sử dụng

### 1. Khởi động ứng dụng

1. Đảm bảo Docker Desktop đang chạy
2. Đảm bảo Ollama đang chạy với mô hình llama2
3. Mở Terminal/Command Prompt, di chuyển vào thư mục src
4. Chạy lệnh: `streamlit run main.py`

### 2. Tải và xử lý dữ liệu

**Cách 1: Từ file JSON local**

1. Chọn tab "File Local" ở thanh bên
2. Nhập đường dẫn thư mục chứa file JSON (mặc định: data)
3. Nhập tên file JSON (mặc định: stack.json)
4. Nhấn "Tải dữ liệu từ file"
5. Đợi hệ thống xử lý và thông báo thành công

**Cách 2: Từ URL**

1. Chọn tab "URL trực tiếp" ở thanh bên
2. Nhập URL cần crawl dữ liệu
3. Nhấn "Crawl dữ liệu"
4. Đợi hệ thống crawl và xử lý dữ liệu

### 3. Tương tác với chatbot

1. Nhập câu hỏi vào ô chat ở phần dưới màn hình
2. Nhấn Enter hoặc nút gửi để gửi câu hỏi
3. Chatbot sẽ:
   - Tìm kiếm thông tin liên quan trong cơ sở dữ liệu
   - Kết hợp kết quả từ nhiều nguồn
   - Tạo câu trả lời dựa trên ngữ cảnh
4. Lịch sử chat sẽ được hiển thị ở phần chính của màn hình

### 4. Xem thông tin hệ thống

- Theo dõi trạng thái kết nối Milvus ở thanh bên
- Kiểm tra số lượng documents đã được tải
- Xem thông tin về mô hình đang sử dụng
