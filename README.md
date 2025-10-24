<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>File Upload Manager</title>
  <style>
    body {
      font-family: "Segoe UI", sans-serif;
      background: #f5f7fa;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
    }
    .container {
      background: #fff;
      width: 90%;
      max-width: 600px;
      margin-top: 50px;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.1);
      padding: 30px;
    }
    h2 {
      text-align: center;
      margin-bottom: 20px;
      color: #333;
    }
    .upload-area {
      border: 2px dashed #6c63ff;
      border-radius: 12px;
      padding: 30px;
      text-align: center;
      color: #777;
      transition: 0.3s;
    }
    .upload-area.dragover {
      background: #f0f0ff;
      border-color: #4a3eff;
      color: #333;
    }
    .upload-area input {
      display: none;
    }
    .file-list {
      margin-top: 20px;
    }
    .file-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #fafafa;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 10px 15px;
      margin-bottom: 10px;
      font-size: 14px;
    }
    .file-item span {
      flex: 1;
    }
    .file-item button {
      background: #ff5c5c;
      border: none;
      color: white;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .file-item button:hover {
      background: #e04040;
    }
    .progress {
      width: 100%;
      background: #e0e0e0;
      border-radius: 5px;
      height: 8px;
      overflow: hidden;
      margin-top: 10px;
    }
    .progress-bar {
      height: 100%;
      background: #6c63ff;
      width: 0%;
      transition: width 0.3s;
    }
    .upload-btn {
      margin-top: 20px;
      background: #6c63ff;
      color: white;
      border: none;
      width: 100%;
      padding: 12px;
      font-size: 16px;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .upload-btn:hover {
      background: #4a3eff;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>📁 File Upload Manager</h2>

    <div class="upload-area" id="uploadArea">
      <p>Drag & Drop files here or click to upload</p>
      <input type="file" id="fileInput" multiple />
    </div>

    <div class="file-list" id="fileList"></div>

    <button class="upload-btn" id="uploadBtn">Upload Files</button>
  </div>

  <script>
    const uploadArea = document.getElementById('uploadArea');
    const fileInput = document.getElementById('fileInput');
    const fileList = document.getElementById('fileList');
    const uploadBtn = document.getElementById('uploadBtn');
    let files = [];

    // File selection
    uploadArea.addEventListener('click', () => fileInput.click());
    fileInput.addEventListener('change', handleFiles);
    uploadArea.addEventListener('dragover', (e) => {
      e.preventDefault();
      uploadArea.classList.add('dragover');
    });
    uploadArea.addEventListener('dragleave', () => uploadArea.classList.remove('dragover'));
    uploadArea.addEventListener('drop', (e) => {
      e.preventDefault();
      uploadArea.classList.remove('dragover');
      handleFiles({ target: { files: e.dataTransfer.files } });
    });

    function handleFiles(e) {
      for (let file of e.target.files) {
        if (!files.some(f => f.name === file.name)) {
          files.push(file);
        }
      }
      displayFiles();
    }

    function displayFiles() {
      fileList.innerHTML = '';
      files.forEach((file, index) => {
        const item = document.createElement('div');
        item.classList.add('file-item');
        item.innerHTML = `
          <span>${file.name} (${(file.size/1024).toFixed(1)} KB)</span>
          <button onclick="removeFile(${index})">Remove</button>
        `;
        fileList.appendChild(item);
      });
    }

    function removeFile(index) {
      files.splice(index, 1);
      displayFiles();
    }

    uploadBtn.addEventListener('click', () => {
      if (files.length === 0) return alert('No files selected!');
      simulateUpload();
    });

    function simulateUpload() {
      fileList.innerHTML = '';
      files.forEach(file => {
        const item = document.createElement('div');
        item.classList.add('file-item');
        item.innerHTML = `
          <span>${file.name}</span>
          <div class="progress"><div class="progress-bar"></div></div>
        `;
        fileList.appendChild(item);

        const progressBar = item.querySelector('.progress-bar');
        let progress = 0;
        const interval = setInterval(() => {
          progress += Math.random() * 15;
          if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
          }
          progressBar.style.width = progress + '%';
        }, 300);
      });
    }
  </script>
</body>
</html>
