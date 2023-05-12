<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- CKEditor -->
    <script src="https://cdn.ckeditor.com/4.16.0/standard/ckeditor.js"></script>
    <style>
        .output-error {
            color: #ff6464;
        }
    </style>

    <title>Code Editor</title>
  </head>
  <body class="bg-dark text-white">
    <div class="container py-5">
        <h1 class="mb-4 text-center">Code Editor</h1>

        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="input-group mb-3">
                    <input type="text" id="url-input" class="form-control" placeholder="Enter URL" aria-label="Enter URL">
                    <button id="write-button" class="btn btn-primary">Fetch Content</button>
                    <button id="clear-button" class="btn btn-secondary">Clear</button>
                </div>

                <textarea name="editor1" id="editor1"></textarea>
            </div>
        </div>
    </div>

    <!-- CKEditor Replace -->
    <script>
        CKEDITOR.replace('editor1');
    </script>

    <!-- Functionality -->
    <script>
        document.getElementById("write-button").addEventListener("click", function() {
            var url = document.getElementById("url-input").value;
            var corsProxy = "https://api.allorigins.win/raw?url=";
            fetch(corsProxy + url)
                .then(response => response.text())
                .then(data => {
                    var parser = new DOMParser();
                    var htmlDoc = parser.parseFromString(data, 'text/html');
                    var headingsAndParagraphs = htmlDoc.querySelectorAll('h1, h2, h3, h4, h5, h6, p');
                    var textContent = '';
                    headingsAndParagraphs.forEach(function(element) {
                        textContent += '<p>' + element.textContent + '</p>';
                    });
                    CKEDITOR.instances.editor1.setData(textContent);
                })
                .catch(error => {
                    console.error(error);
                    CKEDITOR.instances.editor1.setData('<p class="output-error">An error occurred while fetching the content. Please try again with a different URL.</p>');
                });
        });

        document.getElementById("clear-button").addEventListener("click", function() {
            CKEDITOR.instances.editor1.setData('');
            document.getElementById("url-input").value = '';
        });
    </script>

    <!-- Optional JavaScript; choose one of the two! -->
    <!-- Option 1: Bootstrap Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js"></script>

  </body>
</html>
