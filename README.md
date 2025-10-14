
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
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #588157, #3a5a40, #344e41);
            color: #fff;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 2rem;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 2rem;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            flex: 1;
        }
        
        h1 {
            text-align: center;
            margin-bottom: 1.5rem;
            font-size: 2.5rem;
            text-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
            color: #dad7cd;
        }
        
        .description {
            text-align: center;
            margin-bottom: 2rem;
            line-height: 1.6;
            opacity: 0.9;
            color: #dad7cd;
        }
        
        .converter {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }
        
        .input-group, .output-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }
        
        label {
            font-weight: 600;
            font-size: 1.1rem;
            color: #a3b18a;
        }
        
        textarea {
            width: 100%;
            min-height: 120px;
            padding: 1rem;
            border-radius: 10px;
            border: 2px solid #3a5a40;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1rem;
            resize: vertical;
            transition: all 0.3s;
        }
        
        textarea:focus {
            outline: none;
            border-color: #a3b18a;
            box-shadow: 0 0 10px rgba(163, 177, 138, 0.5);
        }
        
        .buttons {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
        }
        
        button {
            flex: 1;
            min-width: 150px;
            padding: 0.8rem 1.5rem;
            border: none;
            border-radius: 10px;
            background: linear-gradient(to right, #588157, #3a5a40);
            color: white;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3);
            background: linear-gradient(to right, #5a8c5a, #3e6344);
        }
        
        button:active {
            transform: translateY(1px);
        }
        
        button:disabled {
            background: #4a5d53;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .morse-output {
            background: rgba(255, 255, 255, 0.1);
            padding: 1rem;
            border-radius: 10px;
            min-height: 120px;
            font-family: monospace;
            font-size: 1.2rem;
            letter-spacing: 2px;
            line-height: 1.6;
            word-break: break-word;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .visualization {
            margin-top: 1rem;
            padding: 1rem;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
            gap: 5px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .dot, .dash, .space {
            display: inline-block;
            transition: all 0.3s;
        }
        
        .dot {
            width: 12px;
            height: 12px;
            background: #a3b18a;
            border-radius: 50%;
            box-shadow: 0 0 10px rgba(163, 177, 138, 0.7);
        }
        
        .dash {
            width: 30px;
            height: 12px;
            background: #a3b18a;
            border-radius: 6px;
            box-shadow: 0 0 10px rgba(163, 177, 138, 0.7);
        }
        
        .space {
            width: 20px;
            height: 5px;
        }
        
        .active {
            transform: scale(1.2);
            background: #dad7cd;
            box-shadow: 0 0 15px #dad7cd;
        }
        
        .instructions {
            margin-top: 2rem;
            padding: 1.5rem;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .instructions h3 {
            margin-bottom: 1rem;
            color: #a3b18a;
        }
        
        .instructions ul {
            list-style-type: none;
            padding-left: 1rem;
        }
        
        .instructions li {
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            color: #dad7cd;
        }
        
        .instructions li:before {
            content: "•";
            color: #a3b18a;
            font-weight: bold;
            margin-right: 0.5rem;
        }
        
        .footer {
            margin-top: 2rem;
            text-align: center;
            color: #dad7cd;
            font-size: 1.2rem;
            font-weight: 500;
            padding: 1rem;
        }
        
        .heart {
            display: inline-block;
            animation: heartbeat 1.5s infinite;
            color: #ff6b6b;
            text-shadow: 0 0 10px rgba(255, 107, 107, 0.7);
        }
        
        @keyframes heartbeat {
            0% {
                transform: scale(1);
            }
            5% {
                transform: scale(1.1);
            }
            10% {
                transform: scale(1);
            }
            15% {
                transform: scale(1.2);
            }
            50% {
                transform: scale(1);
            }
            100% {
                transform: scale(1);
            }
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 1.5rem;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .buttons {
                flex-direction: column;
            }
            
            button {
                min-width: 100%;
            }
            
            .footer {
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Morse Code Converter</h1>
        
        <p class="description">
            Convert text to Morse code and vice versa. You can also play the Morse code as sound and see a visual representation.
        </p>
        
        <div class="converter">
            <div class="input-group">
                <label for="inputText">Enter Text or Morse Code:</label>
                <textarea id="inputText" placeholder="Type your text here..."></textarea>
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
            
            <div class="visualization" id="visualization">
                <div class="instructions">
                    <h3>Morse Code Visualization</h3>
                    <p>Visual representation of your Morse code will appear here when you play the sound.</p>
                </div>
            </div>
        </div>
        
        <div class="instructions">
            <h3>About Morse Code</h3>
            <ul>
                <li>Morse code is a method of transmitting text information as a series of on-off tones, lights, or clicks.</li>
                <li>Each character (letter or numeral) is represented by a unique sequence of dots and dashes.</li>
                <li>International Morse code is composed of five elements: short mark (dot), longer mark (dash), intra-character gap, short gap, and medium gap.</li>
            </ul>
        </div>
    </div>

    <div class="footer">
        Made with <span class="heart">💖</span> By Armeen
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const inputText = document.getElementById('inputText');
            const outputText = document.getElementById('outputText');
            const toMorseBtn = document.getElementById('toMorseBtn');
            const toTextBtn = document.getElementById('toTextBtn');
            const playBtn = document.getElementById('playBtn');
            const clearBtn = document.getElementById('clearBtn');
            const visualization = document.getElementById('visualization');
            
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
                
                // Clear previous visualization
                visualization.innerHTML = '';
                
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
                        visualization.appendChild(createVisualElement('dot', time));
                        time += 0.1;
                        gainNode.gain.setValueAtTime(0, time);
                    } else if (token === '-') {
                        // Dash: beep for 300ms
                        gainNode.gain.setValueAtTime(0.5, time);
                        visualization.appendChild(createVisualElement('dash', time));
                        time += 0.3;
                        gainNode.gain.setValueAtTime(0, time);
                    } else if (token === ' ') {
                        // Space between characters: pause for 200ms
                        visualization.appendChild(createVisualElement('space', time));
                        time += 0.2;
                    } else if (token === '/') {
                        // Space between words: pause for 500ms
                        visualization.appendChild(createVisualElement('space', time));
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
            
            // Create visualization element
            function createVisualElement(type, delay) {
                const element = document.createElement('div');
                element.className = type;
                
                // Animate the element when it's time to play
                setTimeout(() => {
                    element.classList.add('active');
                    setTimeout(() => {
                        element.classList.remove('active');
                    }, (type === 'dot' ? 100 : type === 'dash' ? 300 : 0));
                }, delay * 1000);
                
                return element;
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
                visualization.innerHTML = '<div class="instructions"><h3>Morse Code Visualization</h3><p>Visual representation of your Morse code will appear here when you play the sound.</p></div>';
            });
        });
    </script>
</body>
</html>
