<html lang="en">
<head>
    <title>Content Writer</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.0.0/css/bootstrap.min.css">
  
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        
        .logo {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 20px;
        }
        
        .search-bar {
            margin-bottom: 20px;
        }
        
        .button {
            padding: 5px 30px;
            font-size: 16px;
            height: 40px;
            width: 150px;
        }
        
        .rounded-input {
            border-radius: 30px;
        }
        
        .output {
            margin-top: 20px;
            width: 100%;
            height: 200px;
            overflow: auto;
            border: 1px solid #ccc;
            padding: 10px;
        }
    </style>
</head>
    
<body>
    <div class="logo">SEO Writer</div>
    <div class="search-bar">
        <div class="input-group">
            <input id="url-input" type="text" class="form-control form-control-lg rounded-input" placeholder="Enter URL">
        </div>
    </div>
    <button id="write-button" class="btn btn-primary button">Write</button>
    <div id="output" class="output"></div>

    <!-- JavaScript -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.0.0/js/bootstrap.bundle.min.js"></script>
    <script>
        document.getElementById("write-button").addEventListener("click", function() {
            var url = document.getElementById("url-input").value;
            var corsProxy = "https://api.allorigins.win/raw?url=";
            fetch(corsProxy + url)
                .then(response => response.text())
                .then(data => {
                    // HTML'i ayrıştır
                    var parser = new DOMParser();
                    var htmlDoc = parser.parseFromString(data, 'text/html');
    
                    // Sadece h ve p etiketlerini al
                    var headingsAndParagraphs = htmlDoc.querySelectorAll('h1, h2, h3, h4, h5, h6, p');
                    var textContent = '';
                    headingsAndParagraphs.forEach(function(element) {
                        textContent += '<p>' + element.textContent + '</p>';
                    });
    
                    // Metni görüntüle
                    document.getElementById("output").innerHTML = textContent;
                })
                .catch(error => {
                    console.error(error);
                    document.getElementById("output").textContent = "An error occurred while fetching the content.";
                });
        });
    </script>
</body>
</html>

                   


