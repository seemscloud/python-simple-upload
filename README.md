```bash
export DEST_DIR="/tmp"
```

```python
import json
import os

from typing import List
from fastapi import FastAPI, UploadFile, File
from fastapi.responses import RedirectResponse

app = FastAPI()


@app.get("/")
async def root():
    response = RedirectResponse(url='/docs')
    return response

@app.post("/upload")
async def upload_files(files: List[UploadFile] = File(...)):
  names = []
  for tmp in files:
    names.append(tmp.filename)
    with open(f"{os.environ['DEST_DIR']}/{tmp.filename}", 'wb') as buffer:
      buffer.write(tmp.file.read())

  return {"status": json.dumps(names)}

@app.get("/upload")
async def get_files():
  return os.listdir(f"{os.environ['DEST_DIR']}")
```
