<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scan et Voir</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 20px;
        }
        #camera-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin: 0 auto;
        }
        #camera {
            width: 100%;
            height: auto;
        }
        #image-overlay {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 200px;
            height: 200px;
            object-fit: cover;
            opacity: 0.7;
        }
        #upload-btn {
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <h1>Scan et Voir ton dessin en temps réel</h1>

    <!-- Zone de la caméra -->
    <div id="camera-container">
        <video id="camera" autoplay></video>
        <!-- Le dessin apparaîtra ici -->
        <img id="image-overlay" src="" alt="Image" style="display:none;">
    </div>

    <button id="upload-btn">Choisis ton dessin</button>
    
    <script>
        // Accéder à la caméra de l'utilisateur
        const videoElement = document.getElementById("camera");
        const imageOverlay = document.getElementById("image-overlay");
        const uploadButton = document.getElementById("upload-btn");

        // Démarrer la caméra
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(function(stream) {
                videoElement.srcObject = stream;
            })
            .catch(function(error) {
                console.error("Erreur d'accès à la caméra", error);
            });

        // Fonction pour permettre à l'utilisateur de télécharger l'image du dessin
        uploadButton.addEventListener('click', function() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = 'image/*';
            input.click();

            input.onchange = function(event) {
                const file = event.target.files[0];
                const reader = new FileReader();

                reader.onload = function() {
                    imageOverlay.src = reader.result;
                    imageOverlay.style.display = 'block';
                };

                if (file) {
                    reader.readAsDataURL(file);
                }
            };
        });
    </script>

</body>
</html>
