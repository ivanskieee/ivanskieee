<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Programmer Animation</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #0f0f23, #1a1a2e, #16213e);
            font-family: 'Courier New', monospace;
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .coding-scene {
            position: relative;
            width: 800px;
            height: 600px;
            background: #1e1e1e;
            border-radius: 15px;
            box-shadow: 0 20px 40px rgba(0, 255, 150, 0.2);
            border: 2px solid #00ff96;
            overflow: hidden;
        }

        .window-header {
            background: #2d2d2d;
            height: 40px;
            display: flex;
            align-items: center;
            padding: 0 15px;
            border-bottom: 1px solid #404040;
        }

        .window-controls {
            display: flex;
            gap: 8px;
        }

        .control {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            cursor: pointer;
        }

        .control.red { background: #ff5f57; }
        .control.yellow { background: #ffbd2e; }
        .control.green { background: #28ca42; }

        .window-title {
            flex: 1;
            text-align: center;
            color: #ffffff;
            font-size: 12px;
        }

        .code-container {
            padding: 20px;
            height: calc(100% - 60px);
            background: #1e1e1e;
            position: relative;
        }

        .code-line {
            color: #00ff96;
            font-size: 14px;
            line-height: 1.6;
            margin-bottom: 5px;
            opacity: 0;
            transform: translateX(-20px);
        }

        .comment { color: #6a9955; }
        .keyword { color: #569cd6; }
        .string { color: #ce9178; }
        .function { color: #dcdcaa; }
        .variable { color: #9cdcfe; }
        .operator { color: #d4d4d4; }

        .cursor {
            display: inline-block;
            width: 2px;
            height: 18px;
            background: #00ff96;
            animation: blink 1s infinite;
            margin-left: 2px;
        }

        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0; }
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .typing-animation {
            animation: slideIn 0.5s ease forwards;
        }

        .floating-elements {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
        }

        .floating-bracket {
            position: absolute;
            color: #00ff96;
            font-size: 24px;
            opacity: 0.3;
            animation: float 8s infinite linear;
        }

        @keyframes float {
            from {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 0.3;
            }
            90% {
                opacity: 0.3;
            }
            to {
                transform: translateY(-100px) rotate(360deg);
                opacity: 0;
            }
        }

        .matrix-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0.1;
            z-index: -1;
        }

        .matrix-char {
            position: absolute;
            color: #00ff96;
            font-size: 12px;
            animation: matrixFall 3s infinite linear;
        }

        @keyframes matrixFall {
            to {
                transform: translateY(100vh);
                opacity: 0;
            }
        }

        .programmer-avatar {
            position: absolute;
            bottom: 20px;
            right: 20px;
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: linear-gradient(135deg, #00ff96, #0080ff);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        .status-bar {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 25px;
            background: #007acc;
            display: flex;
            align-items: center;
            padding: 0 15px;
            font-size: 11px;
            color: white;
        }

        .progress-bar {
            width: 100px;
            height: 4px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 2px;
            margin-left: 10px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: #00ff96;
            width: 0%;
            animation: progress 10s infinite;
        }

        @keyframes progress {
            0% { width: 0%; }
            50% { width: 100%; }
            100% { width: 0%; }
        }
    </style>
</head>
<body>
    <div class="coding-scene">
        <div class="window-header">
            <div class="window-controls">
                <div class="control red"></div>
                <div class="control yellow"></div>
                <div class="control green"></div>
            </div>
            <div class="window-title">ivan_brilata_dev.rb - Visual Studio Code</div>
        </div>
        
        <div class="code-container">
            <div class="matrix-bg" id="matrixBg"></div>
            
            <div id="codeLines"></div>
            
            <div class="programmer-avatar">👨‍💻</div>
        </div>

        <div class="status-bar">
            <span>● Ruby on Rails</span>
            <div class="progress-bar">
                <div class="progress-fill"></div>
            </div>
            <span style="margin-left: auto;">Building the future... 🚀</span>
        </div>

        <div class="floating-elements" id="floatingElements"></div>
    </div>

    <script>
        const codeLines = [
            '<span class="comment"># Ivan Brilata - Full Stack Developer</span>',
            '<span class="keyword">class</span> <span class="function">Developer</span>',
            '  <span class="keyword">def</span> <span class="function">initialize</span>',
            '    <span class="variable">@name</span> <span class="operator">=</span> <span class="string">"Ivan Brilata"</span>',
            '    <span class="variable">@skills</span> <span class="operator">=</span> [<span class="string">"Ruby"</span>, <span class="string">"Rails"</span>, <span class="string">"React"</span>, <span class="string">"PHP"</span>]',
            '    <span class="variable">@passion</span> <span class="operator">=</span> <span class="string">"Building amazing web apps"</span>',
            '  <span class="keyword">end</span>',
            '',
            '  <span class="keyword">def</span> <span class="function">create_magic</span>',
            '    <span class="variable">technologies</span> <span class="operator">=</span> {',
            '      <span class="string">frontend:</span> [<span class="string">"React"</span>, <span class="string">"Vite"</span>, <span class="string">"Tailwind"</span>],',
            '      <span class="string">backend:</span> [<span class="string">"Ruby on Rails"</span>, <span class="string">"Laravel"</span>],',
            '      <span class="string">database:</span> [<span class="string">"PostgreSQL"</span>, <span class="string">"MySQL"</span>]',
            '    }',
            '',
            '    <span class="variable">technologies</span>.<span class="function">each</span> <span class="keyword">do</span> |<span class="variable">category</span>, <span class="variable">tools</span>|',
            '      <span class="function">puts</span> <span class="string">"Mastering #{category}: #{tools.join(\', \')}"</span>',
            '    <span class="keyword">end</span>',
            '  <span class="keyword">end</span>',
            '',
            '  <span class="keyword">def</span> <span class="function">contact_info</span>',
            '    {',
            '      <span class="string">email:</span> <span class="string">"ibrilata.dev@gmail.com"</span>,',
            '      <span class="string">linkedin:</span> <span class="string">"ivan-brilata-634a17373"</span>',
            '    }',
            '  <span class="keyword">end</span>',
            '<span class="keyword">end</span>',
            '',
            '<span class="comment"># Let\'s code the future! 🚀</span>',
            '<span class="variable">dev</span> <span class="operator">=</span> <span class="function">Developer</span>.<span class="function">new</span>',
            '<span class="variable">dev</span>.<span class="function">create_magic</span><span class="cursor"></span>'
        ];

        const container = document.getElementById('codeLines');
        let lineIndex = 0;

        function typeLine() {
            if (lineIndex < codeLines.length) {
                const lineDiv = document.createElement('div');
                lineDiv.className = 'code-line typing-animation';
                lineDiv.innerHTML = codeLines[lineIndex];
                container.appendChild(lineDiv);
                
                lineIndex++;
                setTimeout(typeLine, 800);
            } else {
                // Restart animation
                setTimeout(() => {
                    container.innerHTML = '';
                    lineIndex = 0;
                    typeLine();
                }, 3000);
            }
        }

        // Start typing animation
        typeLine();

        // Create floating brackets
        function createFloatingBracket() {
            const brackets = ['{', '}', '(', ')', '[', ']', '<', '>', ';'];
            const bracket = document.createElement('div');
            bracket.className = 'floating-bracket';
            bracket.textContent = brackets[Math.floor(Math.random() * brackets.length)];
            bracket.style.left = Math.random() * 100 + '%';
            bracket.style.animationDelay = Math.random() * 2 + 's';
            
            document.getElementById('floatingElements').appendChild(bracket);
            
            setTimeout(() => {
                bracket.remove();
            }, 8000);
        }

        // Create matrix background
        function createMatrixChar() {
            const chars = '01010101';
            const char = document.createElement('div');
            char.className = 'matrix-char';
            char.textContent = chars[Math.floor(Math.random() * chars.length)];
            char.style.left = Math.random() * 100 + '%';
            char.style.animationDelay = Math.random() * 2 + 's';
            
            document.getElementById('matrixBg').appendChild(char);
            
            setTimeout(() => {
                char.remove();
            }, 3000);
        }

        // Start animations
        setInterval(createFloatingBracket, 1000);
        setInterval(createMatrixChar, 200);
    </script>
</body>
</html>
