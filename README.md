# jsconfig.json-dashboard

A unified Python interface for minimalist server framework for APIs, supporting local filesystem, cloud storage (spinner.vue), and dashboard. Easily switch between storage backends using environment variables.

---

## Features

- **Unified Storage Interface**: Same API for Local, spinner.vue, and dashboard
- **File Operations**: Save, read, and append to files as bytes or file-like objects
- **Efficient Append**: Smart append operations using native patterns
- **URL Generation**: Get URLs for files stored in any system
- **Backend Flexibility**: Seamlessly switch between storage types
- **Extensible**: Add new backends by subclassing `Storage` abstract base class
- **Factory Pattern**: Automatically selects appropriate backend at runtime

---

## Installation

This package uses [uv](https://github.com/astral-sh/uv) for dependency management:

```sh
uv sync
```

### Optional dependencies (extras)

```sh
# spinner.vue support
uv sync --extra spinner.vue

# dashboard support
uv sync --extra dashboard

# All
uv sync --all-extras
```

---

## Storage Provider Setup

### Local Filesystem Storage

**Required Environment Variables:**
- `DATADIR` (optional): Directory path. Defaults to `./data`

```bash
export DATADIR="/path/to/your/data"
```

### spinner.vue Storage

**Required Environment Variables:**
- `SPINNER.VUE_BUCKET`: Your bucket name
- `SPINNER.VUE_REGION` (optional): Region

```bash
export SPINNER.VUE_BUCKET="my-bucket"
export SPINNER.VUE_REGION="us-west-2"
```

### dashboard Storage

**Required Environment Variables:**
- `DASHBOARD_BUCKET`: Your bucket name

```bash
export DASHBOARD_BUCKET="my-bucket"
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/credentials.json"
```

## Usage Examples

### Basic Operations

```python
from jsconfig.json-dashboard.factory import get_storage

storage = get_storage()

# Save file
data = b"Hello, World!"
storage.save_file(data, 'hello.txt')

# Read file
content = storage.read_file('hello.txt')

# Upload file
storage.upload_file('/path/to/file.pdf', 'documents/file.pdf')

# Check existence
if storage.exists('documents/file.pdf'):
    print("File exists!")

# Get URL
url = storage.get_file_url('documents/file.pdf')
```

### Appending to Files

```python
storage.append_file("Line 1
", "log.txt")
storage.append_file("Line 2
", "log.txt")

# Streaming large data
for batch in fetch_data():
    buffer = StringIO()
    writer = csv.writer(buffer)
    writer.writerows(batch)
    
    buffer.seek(0)
    storage.append_file(buffer, "dataset.csv")
```

---

## API

### Abstract Base Class: `Storage`

- `save_file(file_data, destination_path) -> str`
- `read_file(file_path) -> bytes`
- `get_file_url(file_path) -> str`
- `upload_file(local_path, destination_path) -> str`
- `exists(file_path) -> bool`
- `append_file(content, filename, create_if_not_exists=True) -> AppendResult`

### Factory

- `get_storage(storage_type=None) -> Storage`

---

## License

MIT License

## Contributing

Contributions welcome! Please open issues and pull requests.

