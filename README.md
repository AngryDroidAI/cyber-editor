<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cyberpunk Code Editor</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Courier New', monospace;
        }
        
        body {
            background-color: #0a0a1a;
            color: #00ffcc;
            overflow: hidden;
            height: 100vh;
            display: grid;
            grid-template-rows: auto 1fr auto;
            padding: 10px;
            gap: 10px;
        }
        
        /* Header styles */
        header {
            grid-row: 1;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: rgba(0, 20, 40, 0.8);
            border: 1px solid #00ffcc;
            border-radius: 5px;
            box-shadow: 0 0 10px #00ffcc;
        }
        
        .logo {
            font-size: 24px;
            font-weight: bold;
            text-shadow: 0 0 5px #00ffcc;
            letter-spacing: 2px;
        }
        
        /* Editors container */
        .container {
            grid-row: 2;
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            grid-gap: 10px;
            height: 100%;
            overflow: hidden;
        }
        
        /* Editor styles */
        .editor {
            background: rgba(0, 20, 40, 0.8);
            border: 1px solid #00ffcc;
            border-radius: 5px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 10px #00ffcc;
            overflow: hidden;
            position: relative;
        }
        
        .editor-header {
            padding: 10px;
            background: rgba(0, 40, 80, 0.8);
            border-bottom: 1px solid #00ffcc;
            text-align: center;
            font-weight: bold;
            text-shadow: 0 0 5px #00ffcc;
            display: flex;
            justify-content: space-between;
        }
        
        .editor-content {
            flex: 1;
            overflow: auto;
            font-size: 14px;
            line-height: 1.5;
        }
        
        textarea {
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            color: #00ffcc;
            border: none;
            resize: none;
            padding: 15px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            outline: none;
            box-shadow: inset 0 0 50px #00ffcc;
            overflow: auto;
            tab-size: 4;
        }
        
        /* Buttons */
        .buttons {
            display: flex;
            justify-content: center;
            gap: 5px;
            padding: 10px;
            border-top: 1px solid #00ffcc;
        }
        
        button {
            background: rgba(0, 40, 80, 0.8);
            color: #00ffcc;
            border: 1px solid #00ffcc;
            border-radius: 3px;
            padding: 5px 10px;
            cursor: pointer;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            transition: all 0.3s;
            text-shadow: 0 0 5px #00ffcc;
        }
        
        button:hover {
            background: rgba(0, 60, 120, 0.8);
            box-shadow: 0 0 10px #00ffcc;
        }
        
        button.refresh-preview {
            background: rgba(80, 0, 0, 0.8);
            border-color: #ff00cc;
            color: #ff00cc;
        }
        
        button.restore, button.restore-preview {
            background: rgba(0, 80, 0, 0.8);
            border-color: #00ccff;
            color: #00ccff;
        }
        
        button.refresh-preview:hover {
            box-shadow: 0 0 10px #ff00cc;
        }
        
        button.restore:hover, button.restore-preview:hover {
            box-shadow: 0 0 10px #00ccff;
        }
        
        button.hidden {
            display: none;
        }
        
        /* Preview pane */
        .preview {
            grid-row: 3;
            background: rgba(0, 20, 40, 0.8);
            border: 1px solid #00ffcc;
            border-radius: 5px;
            box-shadow: 0 0 10px #00ffcc;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            min-height: 300px;
        }
        
        .preview-header {
            padding: 10px;
            background: rgba(0, 40, 80, 0.8);
            border-bottom: 1px solid #00ffcc;
            text-align: center;
            font-weight: bold;
            text-shadow: 0 0 5px #00ffcc;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .preview-content {
            flex: 1;
            padding: 0;
            overflow: auto;
            background: black;
        }
        
        /* Status bar */
        .status-bar {
            grid-row: 4;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: rgba(0, 20, 40, 0.8);
            border: 1px solid #00ffcc;
            border-radius: 5px;
            box-shadow: 0 0 10px #00ffcc;
            font-size: 12px;
        }
        
        .status-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .status-indicator {
            width: 10px;
            height: 10px;
            background: #00ffcc;
            border-radius: 50%;
            box-shadow: 0 0 10px #00ffcc;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.3; }
            100% { opacity: 1; }
        }
        
        /* Cyberpunk effects */
        .glitch-effect {
            animation: glitch 0.5s;
        }
        
        @keyframes glitch {
            0% { transform: translate(0); }
            2% { transform: translate(-5px, 5px); }
            4% { transform: translate(5px, 5px); }
            6% { transform: translate(-5px, -5px); }
            8% { transform: translate(5px, -5px); }
            10% { transform: translate(0, 0); }
            100% { transform: translate(0, 0); }
        }
        
        /* Expand/restore functionality */
        .expanded {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 100;
            box-shadow: 0 0 30px #00ffcc;
            overflow: auto;
        }
        
        .preview-expanded {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 100;
            box-shadow: 0 0 30px #ff00cc;
            overflow: auto;
        }
        
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <div class="logo">CYBER EDITOR</div>
        <div class="status-bar">
            <div class="status-item">
                <div class="status-indicator"></div>
                <span>System Online</span>
            </div>
            <div class="status-item">Editors: <span id="editor-count">3</span></div>
            <div class="status-item">Preview: <span id="preview-status">Active</span></div>
        </div>
    </header>
    
    <!-- Editors container -->
    <div class="container">
        <!-- HTML Editor -->
        <div class="editor" id="html-editor">
            <div class="editor-header">
                <span>HTML Editor</span>
                <button class="restore hidden" onclick="restoreEditors()">Restore View</button>
            </div>
            <div class="editor-content">
                <textarea id="html-code"><!DOCTYPE html>
<html>
<head>
    <title>Cyberpunk Page</title>
    <style>
        body { background-color: #0a0a1a; }
        .glitch-text { color: #00ffcc; text-shadow: 0 0 5px #00ffcc; }
        .neon-border { border: 2px solid #ff00cc; box-shadow: 0 0 10px #ff00cc; padding: 10px; }
    </style>
</head>
<body>
    <div class="neon-border">
        <h1 class="glitch-text">CYBERPUNK</h1>
        <p>This is a cyberpunk-themed page.</p>
        <button id="neon-btn">HOVER ME</button>
    </div>
</body>
</html></textarea>
            </div>
            <div class="buttons">
                <button onclick="formatHTML()">Format HTML</button>
                <button onclick="expandEditor('html-editor')">Expand</button>
            </div>
        </div>
        
        <!-- CSS Editor -->
        <div class="editor" id="css-editor">
            <div class="editor-header">
                <span>CSS Editor</span>
                <button class="restore hidden" onclick="restoreEditors()">Restore View</button>
            </div>
            <div class="editor-content">
                <textarea id="css-code">body {
    background-color: #0a0a1a;
    margin: 0;
    padding: 0;
    font-family: 'Courier New', monospace;
    color: #00ffcc;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.neon-border {
    border: 2px solid #ff00cc;
    box-shadow: 0 0 10px #ff00cc;
    padding: 20px;
    border-radius: 5px;
}

h1 {
    color: #ff00cc;
    font-size: 3em;
    text-shadow: 0 0 10px #ff00cc;
    animation: glitch 2s infinite;
    margin-bottom: 20px;
}

p {
    font-size: 1.2em;
    margin: 20px 0;
}

#neon-btn {
    background: transparent;
    border: 1px solid #00ffcc;
    color: #00ffcc;
    padding: 10px 20px;
    font-family: 'Courier New', monospace;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s;
    text-transform: uppercase;
}

#neon-btn:hover {
    background: #00ffcc;
    color: #0a0a1a;
    box-shadow: 0 0 20px #00ffcc;
}

@keyframes glitch {
    0% { transform: translate(0); }
    2% { transform: translate(-2px, 2px); }
    4% { transform: translate(2px, 2px); }
    6% { transform: translate(-2px, -2px); }
    8% { transform: translate(2px, -2px); }
    10% { transform: translate(0, 0); }
    100% { transform: translate(0, 0); }
}</textarea>
            </div>
            <div class="buttons">
                <button onclick="formatCSS()">Format CSS</button>
                <button onclick="expandEditor('css-editor')">Expand</button>
            </div>
        </div>
        
        <!-- JavaScript Editor -->
        <div class="editor" id="js-editor">
            <div class="editor-header">
                <span>JavaScript Editor</span>
                <button class="restore hidden" onclick="restoreEditors()">Restore View</button>
            </div>
            <div class="editor-content">
                <textarea id="js-code">document.addEventListener('DOMContentLoaded', function() {
    // Glitch text effect
    const glitchText = document.querySelector('.glitch-text');
    if (glitchText) {
        setInterval(() => {
            glitchText.style.textShadow = `0 0 10px #00ffcc, 0 0 20px #ff00cc`;
            setTimeout(() => {
                glitchText.style.textShadow = `0 0 5px #00ffcc, 0 0 10px #ff00cc`;
            }, 100);
        }, 2000);
    }
    
    // Neon button effect
    const neonBtn = document.getElementById('neon-btn');
    if (neonBtn) {
        neonBtn.addEventListener('mouseenter', function() {
            this.style.boxShadow = '0 0 20px #00ffcc, 0 0 30px #ff00cc';
        });
        
        neonBtn.addEventListener('mouseleave', function() {
            this.style.boxShadow = '0 0 10px #00ffcc';
        });
        
        neonBtn.addEventListener('click', function() {
            document.body.style.animation = 'none';
            document.body.offsetHeight; // Trigger reflow
            document.body.style.animation = 'glitch 0.5s';
        });
    }
});</textarea>
            </div>
            <div class="buttons">
                <button onclick="formatJS()">Format JS</button>
                <button onclick="expandEditor('js-editor')">Expand</button>
            </div>
        </div>
    </div>
    
    <!-- Preview pane -->
    <div class="preview" id="preview-pane">
        <div class="preview-header">
            <span>Live Preview</span>
            <div>
                <button class="refresh-preview" onclick="updatePreview()">Refresh Preview</button>
                <button class="restore-preview hidden" onclick="restorePreview()">Restore View</button>
                <button class="expand-preview" onclick="expandPreview()">Expand Preview</button>
            </div>
        </div>
        <div class="preview-content">
            <iframe id="preview-frame" style="width: 100%; height: 100%; border: none;"></iframe>
        </div>
    </div>
    
    <script>
        // Format HTML code
        function formatHTML() {
            const htmlCode = document.getElementById('html-code').value;
            // Simple HTML formatting with indentation
            let formattedHTML = htmlCode
                .replace(/(>)(<)(\/*)/g, '$1\n$2$3')
                .replace(/(\s+)?<\/?([a-zA-Z]+)([^>]*)?>/g, function(match, p1, p2, p3) {
                    return '\n<' + p2 + (p3 || '') + '>';
                })
                .split('\n')
                .map(line => {
                    // Add indentation based on tag level
                    const indentLevel = (line.match(/<\/?[a-zA-Z]/g) || []).length;
                    return '  '.repeat(indentLevel) + line.trim();
                })
                .join('\n')
                .trim();
                
            document.getElementById('html-code').value = formattedHTML;
            updatePreview();
        }
        
        // Format CSS code
        function formatCSS() {
            const cssCode = document.getElementById('css-code').value;
            // Basic CSS formatting
            let formattedCSS = cssCode
                .replace(/{/g, '{\n  ')
                .replace(/}/g, '\n}\n')
                .replace(/;/g, ';\n  ')
                .replace(/\n\s*\n/g, '\n');
                
            document.getElementById('css-code').value = formattedCSS;
            updatePreview();
        }
        
        // Format JavaScript code
        function formatJS() {
            const jsCode = document.getElementById('js-code').value;
            // Basic JavaScript formatting
            let formattedJS = jsCode
                .replace(/{/g, '{\n  ')
                .replace(/}/g, '\n}\n')
                .replace(/;/g, ';\n')
                .replace(/\n\s*\n/g, '\n');
                
            document.getElementById('js-code').value = formattedJS;
            updatePreview();
        }
        
        // Expand editor functionality
        function expandEditor(editorId) {
            const editors = document.querySelectorAll('.editor');
            const preview = document.querySelector('.preview');
            const header = document.querySelector('header');
            
            // Hide all editors and preview
            editors.forEach(editor => {
                editor.classList.add('hidden');
            });
            preview.classList.add('hidden');
            header.classList.add('hidden');
            
            // Find the clicked editor and expand it
            const editorToExpand = document.getElementById(editorId);
            editorToExpand.classList.remove('hidden');
            editorToExpand.classList.add('expanded');
            
            // Show the restore button
            const restoreBtn = editorToExpand.querySelector('.restore');
            restoreBtn.classList.remove('hidden');
            
            // Update editor count
            document.getElementById('editor-count').textContent = '1 (Expanded)';
            document.getElementById('preview-status').textContent = 'Hidden';
        }
        
        // Restore editors to normal view
        function restoreEditors() {
            const editors = document.querySelectorAll('.editor');
            const preview = document.querySelector('.preview');
            const header = document.querySelector('header');
            
            // Remove expanded class and show all editors and preview
            editors.forEach(editor => {
                editor.classList.remove('expanded');
                editor.classList.remove('hidden');
                // Hide restore buttons
                const restoreBtn = editor.querySelector('.restore');
                restoreBtn.classList.add('hidden');
            });
            preview.classList.remove('hidden');
            header.classList.remove('hidden');
            
            // Update editor count
            document.getElementById('editor-count').textContent = editors.length;
            document.getElementById('preview-status').textContent = 'Active';
        }
        
        // Expand preview to full screen
        function expandPreview() {
            const editors = document.querySelectorAll('.editor');
            const preview = document.querySelector('.preview');
            const header = document.querySelector('header');
            
            // Hide all editors and header
            editors.forEach(editor => {
                editor.classList.add('hidden');
            });
            header.classList.add('hidden');
            
            // Expand the preview
            preview.classList.add('preview-expanded');
            
            // Hide the expand button, show the restore button
            const expandBtn = preview.querySelector('.expand-preview');
            const restoreBtn = preview.querySelector('.restore-preview');
            expandBtn.classList.add('hidden');
            restoreBtn.classList.remove('hidden');
            
            // Update status
            document.getElementById('editor-count').textContent = '0';
            document.getElementById('preview-status').textContent = 'Full Screen';
            
            // Update preview to full size
            updatePreview();
        }
        
        // Restore preview to normal view
        function restorePreview() {
            const editors = document.querySelectorAll('.editor');
            const preview = document.querySelector('.preview');
            const header = document.querySelector('header');
            
            // Remove expanded class and show all elements
            editors.forEach(editor => {
                editor.classList.remove('hidden');
            });
            header.classList.remove('hidden');
            preview.classList.remove('preview-expanded');
            
            // Hide the restore button, show the expand button
            const expandBtn = preview.querySelector('.expand-preview');
            const restoreBtn = preview.querySelector('.restore-preview');
            expandBtn.classList.remove('hidden');
            restoreBtn.classList.add('hidden');
            
            // Update status
            document.getElementById('editor-count').textContent = editors.length;
            document.getElementById('preview-status').textContent = 'Active';
            
            // Update preview
            updatePreview();
        }
        
        // Update preview
        function updatePreview() {
            const htmlCode = document.getElementById('html-code').value;
            const cssCode = document.getElementById('css-code').value;
            const jsCode = document.getElementById('js-code').value;
            
            const previewFrame = document.getElementById('preview-frame');
            const previewDocument = previewFrame.contentDocument || previewFrame.contentWindow.document;
            
            // Extract HTML structure from the HTML code
            const htmlMatch = htmlCode.match(/<body[^>]*>([\s\S]*)<\/body>/i);
            const bodyContent = htmlMatch ? htmlMatch[1] : htmlCode;
            
            // Extract CSS from style tags in HTML and combine with CSS editor content
            const styleMatch = htmlCode.match(/<style[^>]*>([\s\S]*?)<\/style>/i);
            const htmlStyles = styleMatch ? styleMatch[1] : '';
            const combinedCSS = htmlStyles + '\n' + cssCode;
            
            previewDocument.open();
            previewDocument.write(`
                <!DOCTYPE html>
                <html>
                <head>
                    <meta charset="UTF-8">
                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                    <style>
                        ${combinedCSS}
                    </style>
                </head>
                <body>
                    ${bodyContent}
                    <script>
                        ${jsCode}
                    <\/script>
                </body>
                </html>
            `);
            previewDocument.close();
        }
        
        // Initial preview update
        updatePreview();
        
        // Add keyboard shortcuts
        document.addEventListener('keydown', function(event) {
            // Ctrl+Shift+H to format HTML
            if (event.ctrlKey && event.shiftKey && event.key === 'H') {
                event.preventDefault();
                formatHTML();
            }
            // Ctrl+Shift+C to format CSS
            else if (event.ctrlKey && event.shiftKey && event.key === 'C') {
                event.preventDefault();
                formatCSS();
            }
            // Ctrl+Shift+J to format JS
            else if (event.ctrlKey && event.shiftKey && event.key === 'J') {
                event.preventDefault();
                formatJS();
            }
            // Ctrl+R to refresh preview
            else if (event.ctrlKey && event.key === 'r') {
                event.preventDefault();
                updatePreview();
            }
            // Ctrl+E to expand preview (new shortcut)
            else if (event.ctrlKey && event.key === 'e') {
                event.preventDefault();
                const previewPane = document.getElementById('preview-pane');
                if (previewPane.classList.contains('preview-expanded')) {
                    restorePreview();
                } else {
                    expandPreview();
                }
            }
        });
        
        // Auto-update preview on code changes with a slight delay
        let updateTimeout;
        document.querySelectorAll('textarea').forEach(textarea => {
            textarea.addEventListener('input', function() {
                clearTimeout(updateTimeout);
                updateTimeout = setTimeout(updatePreview, 500);
            });
        });
    </script>
</body>
</html>
