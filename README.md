
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Morse Code Converter</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
        }
        
        body {
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .container {
            max-width: 600px;
            width: 100%;
            background: white;
            border-radius: 8px;
            padding: 30px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .logo {
            text-align: center;
            margin-bottom: 30px;
            padding: 10px;
        }
        
        .logo img {
            max-width: 280px;
            height: auto;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .input-group, .output-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #2c3e50;
        }
        
        textarea {
            width: 100%;
            min-height: 100px;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
            resize: vertical;
            transition: border 0.3s;
        }
        
        textarea:focus {
            outline: none;
            border-color: #3498db;
        }
        
        .buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        button {
            flex: 1;
            min-width: 120px;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            background-color: #3498db;
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        button:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        button:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .morse-output {
            background-color: #f8f9fa;
            padding: 12px;
            border-radius: 4px;
            min-height: 100px;
            font-family: monospace;
            font-size: 1.1rem;
            letter-spacing: 1px;
            border: 1px solid #eee;
        }
        
        .footer {
            margin-top: 20px;
            text-align: center;
            color: #7f8c8d;
            font-size: 0.9rem;
        }
        
        .heart {
            color: #e74c3c;
            display: inline-block;
            animation: heartbeat 1.5s infinite;
        }
        
        @keyframes heartbeat {
            0% { transform: scale(1); }
            5% { transform: scale(1.1); }
            10% { transform: scale(1); }
            15% { transform: scale(1.2); }
            50% { transform: scale(1); }
            100% { transform: scale(1); }
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            .logo img {
                max-width: 200px;
            }
            
            .buttons {
                flex-direction: column;
            }
            
            button {
                min-width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">
            <img src="https://i.ibb.co/Dgz38sjY/image.png" alt="Morse Code Logo" border="0">
        </div>
        
        <div class="input-group">
            <label for="inputText">Enter Text or Morse Code:</label>
            <textarea id="inputText" placeholder="Type your text or Morse code here..."></textarea>
        </div>
        
        <div class="buttons">
            <button id="toMorseBtn">Convert to Morse</button>
            <button id="toTextBtn">Convert to Text</button>
            <button id="playBtn">Play Morse Code</button>
            <button id="clearBtn">Clear</button>
        </div>
        
        <div class="output-group">
            <label for="outputText">Converted Output:</label>
            <div id="outputText" class="morse-output">Your converted text will appear here...</div>
        </div>
    </div>

    <div class="footer">
        Made with <span class="heart">❤️</span> By Armeen
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const inputText = document.getElementById('inputText');
            const outputText = document.getElementById('outputText');
            const toMorseBtn = document.getElementById('toMorseBtn');
            const toTextBtn = document.getElementById('toTextBtn');
            const playBtn = document.getElementById('playBtn');
            const clearBtn = document.getElementById('clearBtn');
            
            // Morse code dictionary
            const morseCode = {
                'A': '.-', 'B': '-...', 'C': '-.-.', 'D': '-..', 'E': '.', 'F': '..-.', 
                'G': '--.', 'H': '....', 'I': '..', 'J': '.---', 'K': '-.-', 'L': '.-..', 
                'M': '--', 'N': '-.', 'O': '---', 'P': '.--.', 'Q': '--.-', 'R': '.-.', 
                'S': '...', 'T': '-', 'U': '..-', 'V': '...-', 'W': '.--', 'X': '-..-', 
                'Y': '-.--', 'Z': '--..', '0': '-----', '1': '.----', '2': '..---', 
                '3': '...--', '4': '....-', '5': '.....', '6': '-....', '7': '--...', 
                '8': '---..', '9': '----.', '.': '.-.-.-', ',': '--..--', '?': '..--..', 
                "'": '.----.', '!': '-.-.--', '/': '-..-.', '(': '-.--.', ')': '-.--.-', 
                '&': '.-...', ':': '---...', ';': '-.-.-.', '=': '-...-', '+': '.-.-.', 
                '-': '-....-', '_': '..--.-', '"': '.-..-.', '$': '...-..-', '@': '.--.-.', 
                ' ': '/'
            };
            
            // Reverse dictionary for Morse to text conversion
            const textFromMorse = {};
            for (const [key, value] of Object.entries(morseCode)) {
                textFromMorse[value] = key;
            }
            
            // Convert text to Morse code
            function textToMorse(text) {
                return text.toUpperCase().split('').map(char => {
                    return morseCode[char] || char;
                }).join(' ');
            }
            
            // Convert Morse code to text
            function morseToText(morse) {
                return morse.split(' ').map(code => {
                    return textFromMorse[code] || code;
                }).join('');
            }
            
            // Check if text is Morse code
            function isMorseCode(text) {
                const morseChars = ['.', '-', '/', ' '];
                return text.split('').every(char => morseChars.includes(char));
            }
            
            // Play Morse code as sound
            function playMorseCode(morse) {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                let time = 0;
                
                // Create oscillator
                const oscillator = audioContext.createOscillator();
                oscillator.type = 'sine';
                oscillator.frequency.value = 600; // Hz
                
                // Create gain node for volume control
                const gainNode = audioContext.createGain();
                gainNode.gain.value = 0;
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.start();
                
                // Parse Morse code and schedule beeps
                const tokens = morse.split('');
                
                tokens.forEach(token => {
                    if (token === '.') {
                        // Dot: beep for 100ms
                        gainNode.gain.setValueAtTime(0.5, time);
                        time += 0.1;
                        gainNode.gain.setValueAtTime(0, time);
                    } else if (token === '-') {
                        // Dash: beep for 300ms
                        gainNode.gain.setValueAtTime(0.5, time);
                        time += 0.3;
                        gainNode.gain.setValueAtTime(0, time);
                    } else if (token === ' ') {
                        // Space between characters: pause for 200ms
                        time += 0.2;
                    } else if (token === '/') {
                        // Space between words: pause for 500ms
                        time += 0.5;
                    }
                    
                    // Short pause between elements
                    time += 0.1;
                });
                
                // Stop oscillator after all beeps
                oscillator.stop(time + 0.1);
                
                // Re-enable play button when done
                setTimeout(() => {
                    playBtn.disabled = false;
                    playBtn.textContent = 'Play Morse Code';
                }, time * 1000);
            }
            
            // Event listeners
            toMorseBtn.addEventListener('click', function() {
                const text = inputText.value.trim();
                if (text) {
                    outputText.textContent = textToMorse(text);
                } else {
                    outputText.textContent = 'Please enter some text to convert.';
                }
            });
            
            toTextBtn.addEventListener('click', function() {
                const text = inputText.value.trim();
                if (text) {
                    if (isMorseCode(text)) {
                        outputText.textContent = morseToText(text);
                    } else {
                        outputText.textContent = 'The input does not appear to be valid Morse code.';
                    }
                } else {
                    outputText.textContent = 'Please enter Morse code to convert.';
                }
            });
            
            playBtn.addEventListener('click', function() {
                const morse = outputText.textContent;
                if (morse && morse !== 'Your converted text will appear here...' && 
                    morse !== 'Please enter some text to convert.' && 
                    morse !== 'Please enter Morse code to convert.' && 
                    morse !== 'The input does not appear to be valid Morse code.') {
                    
                    playBtn.disabled = true;
                    playBtn.textContent = 'Playing...';
                    playMorseCode(morse);
                } else {
                    outputText.textContent = 'Please convert text to Morse code first.';
                }
            });
            
            clearBtn.addEventListener('click', function() {
                inputText.value = '';
                outputText.textContent = 'Your converted text will appear here...';
            });
        });
    </script>
</body>
</html>
