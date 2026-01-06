# student-note-app
it is a note taking website especially for maths so that students can take notes with any kind of equation.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    
    <!-- SEO: Essential for Google Search indexing -->
    <title>MathNotes Pro | Student Note App by AKshaj GANDHI</title>
    <meta name="description" content="A professional student note-taking app with math support. Created and designed by AKshaj GANDHI.">
    <meta name="keywords" content="student note app, math notes app, AKshaj GANDHI, study tools, equation editor">
    <meta name="author" content="AKshaj GANDHI">

    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- MathJax for high-quality math rendering -->
    <script>
        window.MathJax = {
            tex: { inlineMath: [['$', '$'], ['\\(', '\\)']], displayMath: [['$$', '$$']] },
            svg: { fontCache: 'global' }
        };
    </script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    
    <style>
        body { -webkit-tap-highlight-color: transparent; }
        .note-card { @apply bg-white p-6 rounded-3xl shadow-sm border border-slate-100 transition-all flex flex-col h-full cursor-pointer; }
        .note-card:hover { transform: translateY(-5px); @apply shadow-2xl border-indigo-200; }
        .signature-text {
            background: linear-gradient(90deg, #4f46e5, #9333ea);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        #noteModal.hidden { display: none; }
        #noteModal { display: flex; animation: slideUp 0.3s ease-out; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>
</head>
<body class="bg-slate-50 min-h-screen font-sans flex flex-col">

    <div id="app" class="max-w-5xl mx-auto p-5 md:p-10 flex-grow w-full">
        <header class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-5">
            <div>
                <h1 class="text-4xl font-black text-slate-900 tracking-tight">MathNotes Pro</h1>
                <p class="text-slate-500 font-medium text-sm">Official App by <span class="font-bold text-indigo-600">AKshaj GANDHI</span></p>
            </div>
            <button onclick="openNoteModal()" class="w-full md:w-auto bg-indigo-600 text-white px-10 py-4 rounded-2xl font-bold shadow-xl shadow-indigo-100 hover:bg-indigo-700 active:scale-95 transition-all text-lg">
                + Create Note
            </button>
        </header>

        <!-- Search Bar -->
        <div class="mb-8 relative">
            <input type="text" id="searchInput" oninput="renderNotes()" placeholder="Search your notes..." class="w-full pl-14 pr-6 py-5 rounded-2xl border-none bg-white shadow-sm focus:ring-2 focus:ring-indigo-500/20 outline-none text-lg">
            <svg class="h-6 w-6 absolute left-5 top-5 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
        </div>

        <div id="notesContainer" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Notes load here -->
        </div>
    </div>

    <footer class="w-full py-10 bg-white border-t border-slate-100 mt-auto text-center">
        <p class="text-slate-400 text-xs font-bold uppercase tracking-[0.3em] mb-2">Developed By</p>
        <h2 class="text-2xl font-black signature-text">AKshaj GANDHI</h2>
        <p class="text-slate-300 text-[10px] mt-4">© 2024 MathNotes Pro. All Rights Reserved.</p>
    </footer>

    <!-- Editor Modal -->
    <div id="noteModal" class="fixed inset-0 bg-slate-900/40 backdrop-blur-md hidden z-50 items-center justify-center p-4">
        <div class="bg-white w-full max-w-2xl max-h-[90vh] overflow-hidden shadow-2xl flex flex-col rounded-[2rem]">
            <div class="p-6 border-b flex justify-between items-center">
                <h2 id="modalTitleText" class="text-2xl font-bold text-slate-800">New Note</h2>
                <button onclick="closeNoteModal()" class="text-3xl text-slate-400 hover:text-slate-600">&times;</button>
            </div>
            <div class="p-6 overflow-y-auto flex-grow space-y-6">
                <input type="text" id="noteTitle" placeholder="Note Title" class="w-full p-2 border-b-2 border-slate-100 focus:border-indigo-500 outline-none text-xl font-bold text-slate-800">
                <textarea id="noteText" placeholder="Write your thoughts... use the buttons below to insert math formulas." oninput="updatePreview()" class="w-full h-40 p-4 border border-slate-100 rounded-2xl focus:ring-2 focus:ring-indigo-500/10 outline-none resize-none bg-slate-50 text-slate-700"></textarea>
                
                <div class="bg-indigo-50/50 p-5 rounded-2xl border border-indigo-100">
                    <p class="text-indigo-900 font-bold text-xs uppercase mb-3">Insert Math Shortcut</p>
                    <div class="flex flex-wrap gap-2">
                        <button onclick="addMath('frac')" class="px-4 py-2 bg-white border border-indigo-200 rounded-xl text-sm font-bold text-indigo-700 shadow-sm active:scale-95">Fraction</button>
                        <button onclick="addMath('sqrt')" class="px-4 py-2 bg-white border border-indigo-200 rounded-xl text-sm font-bold text-indigo-700 shadow-sm active:scale-95">Square Root</button>
                        <button onclick="addMath('pow')" class="px-4 py-2 bg-white border border-indigo-200 rounded-xl text-sm font-bold text-indigo-700 shadow-sm active:scale-95">Power</button>
                    </div>
                </div>

                <div id="livePreview" class="p-5 bg-slate-50 rounded-2xl border-2 border-dashed border-slate-200 min-h-[80px] text-slate-600">
                    <span class="italic text-slate-300">Live preview...</span>
                </div>
            </div>
            <div class="p-6 border-t bg-slate-50">
                <button onclick="saveNote()" id="saveBtn" class="w-full py-4 bg-indigo-600 text-white rounded-2xl font-bold text-lg shadow-lg shadow-indigo-200">Save Note</button>
            </div>
        </div>
    </div>

    <script>
        let notes = JSON.parse(localStorage.getItem('akshaj_pro_notes') || '[]');
        let editingId = null;

        function openNoteModal(id = null) {
            editingId = id;
            const modal = document.getElementById('noteModal');
            if(id) {
                const note = notes.find(n => n.id === id);
                document.getElementById('noteTitle').value = note.title;
                document.getElementById('noteText').value = note.content;
                document.getElementById('modalTitleText').innerText = "Edit Note";
            } else {
                document.getElementById('noteTitle').value = '';
                document.getElementById('noteText').value = '';
                document.getElementById('modalTitleText').innerText = "New Note";
            }
            modal.classList.remove('hidden');
            updatePreview();
        }

        function closeNoteModal() { document.getElementById('noteModal').classList.add('hidden'); }

        function updatePreview() {
            const t = document.getElementById('noteText').value;
            const p = document.getElementById('livePreview');
            p.innerText = t || 'Live preview...';
            MathJax.typesetPromise([p]);
        }

        function addMath(type) {
            const area = document.getElementById('noteText');
            if(type === 'frac') area.value += ' $\\frac{a}{b}$ ';
            else if(type === 'sqrt') area.value += ' $\\sqrt{x}$ ';
            else if(type === 'pow') area.value += ' $a^{b}$ ';
            updatePreview();
        }

        function saveNote() {
            const title = document.getElementById('noteTitle').value.trim();
            const content = document.getElementById('noteText').value.trim();
            if(!title || !content) return;
            if(editingId) {
                const i = notes.findIndex(n => n.id === editingId);
                notes[i] = { ...notes[i], title, content };
            } else {
                notes.unshift({ id: Date.now(), title, content, date: new Date().toLocaleDateString() });
            }
            localStorage.setItem('akshaj_pro_notes', JSON.stringify(notes));
            renderNotes();
            closeNoteModal();
        }

        function renderNotes() {
            const container = document.getElementById('notesContainer');
            const search = document.getElementById('searchInput').value.toLowerCase();
            container.innerHTML = '';
            
            const filtered = notes.filter(n => n.title.toLowerCase().includes(search) || n.content.toLowerCase().includes(search));
            
            if(filtered.length === 0) {
                container.innerHTML = `<div class="col-span-full py-20 text-center text-slate-300">No notes found. Start by creating one!</div>`;
                return;
            }

            filtered.forEach(note => {
                const d = document.createElement('div');
                d.className = "note-card";
                d.onclick = (e) => { if(e.target.tagName !== 'BUTTON') openNoteModal(note.id); };
                d.innerHTML = `
                    <div class="flex justify-between items-start mb-4">
                        <h3 class="font-bold text-xl text-slate-800 leading-tight">${note.title}</h3>
                        <span class="text-[10px] bg-slate-100 text-slate-500 px-2 py-1 rounded-md font-bold">${note.date}</span>
                    </div>
                    <div class="text-sm text-slate-600 flex-grow whitespace-pre-wrap mb-6 line-clamp-4">${note.content}</div>
                    <div class="mt-auto pt-4 border-t border-slate-50 flex justify-between items-center">
                        <span class="text-[10px] text-indigo-500 font-bold uppercase tracking-widest">Tap to view</span>
                        <button onclick="deleteNote(${note.id})" class="text-slate-300 hover:text-red-500 transition-colors">
                            <svg class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" stroke-width="2"/></svg>
                        </button>
                    </div>
                `;
                container.appendChild(d);
            });
            MathJax.typesetPromise([container]);
        }

        function deleteNote(id) {
            if(confirm("Are you sure you want to delete this note?")) {
                notes = notes.filter(n => n.id !== id);
                localStorage.setItem('akshaj_pro_notes', JSON.stringify(notes));
                renderNotes();
            }
        }

        window.onload = renderNotes;
    </script>
</body>
</html>
