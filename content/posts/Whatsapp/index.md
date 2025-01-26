---
title: "Whatsapp"
discription: Whatsapp script
date: 2024-12-13T21:29:01+08:00 
draft: false
type: post
tags: ["Script", "Javascript"]
showTableOfContents: true
--- 





### Whatsapp Link Generator


```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WhatsApp Link Generator</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            height: 100vh;
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: black;
            color: white;
        }
        .ascii-art {
            font-family: monospace;
            white-space: pre;
            line-height: 1.2;
            margin-bottom: 20px;
            color: lime;
        }
        label, input, button, p {
            margin-top: 10px;
        }
        input, button {
            background-color: white;
            color: black;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
        }
        button:hover {
            background-color: gray;
            color: white;
        }
        a {
            color: cyan;
        }
    </style>
</head>
<body>
    <h1>WhatsApp Link Generator</h1>
    <div class="ascii-art">
 /$$      /$$ /$$                   /$$               /$$$$$$                     
| $$  /$ | $$| $$                  | $$              /$$__  $$                    
| $$ /$$$| $$| $$$$$$$   /$$$$$$  /$$$$$$   /$$$$$$$| $$  \ $$  /$$$$$$   /$$$$$$ 
| $$/$$ $$ $$| $$__  $$ |____  $$|_  $$_/  /$$_____/| $$$$$$$$ /$$__  $$ /$$__  $$
| $$$$_  $$$$| $$  \ $$  /$$$$$$$  | $$   |  $$$$$$ | $$__  $$| $$  \ $$| $$  \ $$
| $$$/ \  $$$| $$  | $$ /$$__  $$  | $$ /$$\____  $$| $$  | $$| $$  | $$| $$  | $$
| $$/   \  $$| $$  | $$|  $$$$$$$  |  $$$$//$$$$$$$/| $$  | $$| $$$$$$$/| $$$$$$$/
|__/     \__/|__/  |__/ \_______/   \___/ |_______/ |__/  |__/| $$____/ | $$____/ 
                                                              | $$      | $$      
                                                              | $$      | $$      
                                                              |__/      |__/      
    </div>
    <label for="phoneNumber">Enter the phone number (in the format 972XXXXXXXXXXXXXXX):</label>
    <input type="text" id="phoneNumber" placeholder="972XXXXXXXXX">
    <button onclick="generateLink()">Create link</button>
    <p id="output"></p>

    <script>
        function generateLink() {
            const phoneInput = document.getElementById('phoneNumber');
            let phoneNumber = phoneInput.value.trim();

            if (!/^[0-9]{11,15}$/.test(phoneNumber)) {
                alert('Please enter the correct phone number in international format (without +).');
                return;
            }

            if (phoneNumber.startsWith('9720') && phoneNumber[4] === '5') {
                phoneNumber = '972' + phoneNumber.slice(4);
            }

            const link = `https://web.whatsapp.com/send/?phone=${phoneNumber}&text&type=phone_number&app_absent=0`;
            const outputElement = document.getElementById('output');

            outputElement.innerHTML = `Link: <a href="${link}" target="_blank">Open WhatsApp</a>`;
        }
    </script>
</body>
</html>
```