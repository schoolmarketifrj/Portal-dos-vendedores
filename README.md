<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel do Vendedor - SchoolMarket</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f3f4f6; }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); }
    </style>
</head>
<body class="min-h-screen">

    <!-- Loading Overlay -->
    <div id="loading-overlay" class="fixed inset-0 bg-white z-50 flex flex-col items-center justify-center">
        <div class="animate-spin rounded-full h-12 w-12 border-4 border-blue-600 border-t-transparent mb-4"></div>
        <p class="text-slate-600 font-medium">A carregar painel seguro...</p>
    </div>

    <!-- Auth Screen -->
    <div id="auth-screen" class="hidden min-h-screen flex items-center justify-center p-4">
        <div class="max-w-md w-full glass p-8 rounded-2xl shadow-xl border border-white">
            <div class="text-center mb-8">
                <div class="bg-blue-600 w-16 h-16 rounded-2xl flex items-center justify-center mx-auto mb-4 shadow-lg">
                    <i class="fas fa-store text-white text-3xl"></i>
                </div>
                <h1 id="auth-title" class="text-2xl font-bold text-slate-900">Portal do Vendedor</h1>
                <p id="auth-subtitle" class="text-slate-500">Gestão de produtos SchoolMarket</p>
            </div>

            <form id="auth-form" class="space-y-4">
                <div id="register-fields" class="hidden space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Nome da Minha Loja</label>
                        <input type="text" id="auth-store-name" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-blue-500 outline-none transition-all" placeholder="Ex: Cantinho do Doce">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">WhatsApp (com DDD)</label>
                        <input type="text" id="auth-whatsapp" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-blue-500 outline-none transition-all" placeholder="Ex: 11999999999">
                    </div>
                </div>
                
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">E-mail da Loja</label>
                    <input type="email" id="auth-email" required class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-blue-500 outline-none transition-all" placeholder="vendedor@escola.com">
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">Senha</label>
                    <input type="password" id="auth-password" required class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:ring-2 focus:ring-blue-500 outline-none transition-all" placeholder="••••••••">
                </div>
                <button type="submit" id="auth-submit-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-xl shadow-lg transition-all transform hover:scale-[1.01]">
                    Entrar no Painel
                </button>
            </form>
            
            <div class="mt-6 text-center">
                <button id="toggle-auth" class="text-sm text-blue-600 font-semibold hover:underline">Ainda não tem conta? Registar Loja</button>
            </div>
        </div>
    </div>

    <!-- Main Dashboard -->
    <div id="dashboard" class="hidden flex h-screen overflow-hidden">
        <!-- Sidebar -->
        <aside class="w-64 bg-white shadow-xl flex-shrink-0 flex flex-col hidden md:flex border-r border-slate-100">
            <div class="p-6 border-b flex items-center space-x-3">
                <div class="bg-blue-600 p-2 rounded-lg"><i class="fas fa-box-open text-white"></i></div>
                <span class="font-bold text-xl text-blue-900 tracking-tight">SellerCenter</span>
            </div>
            <nav class="flex-1 p-4 space-y-2 mt-4">
                <a href="#" class="flex items-center space-x-3 p-3 rounded-xl bg-blue-50 text-blue-700 font-medium transition-all">
                    <i class="fas fa-th-large"></i>
                    <span>Meus Produtos</span>
                </a>
            </nav>
            <div class="p-4 border-t space-y-2">
                <button onclick="logout()" class="flex items-center space-x-3 p-3 w-full text-slate-600 hover:bg-slate-50 rounded-xl transition-all">
                    <i class="fas fa-sign-out-alt"></i>
                    <span class="font-medium">Sair da Sessão</span>
                </button>
                <button onclick="confirmDeleteAccount()" class="flex items-center space-x-3 p-3 w-full text-red-500 hover:bg-red-50 rounded-xl transition-all">
                    <i class="fas fa-user-slash text-sm"></i>
                    <span class="font-medium">Excluir Minha Loja</span>
                </button>
            </div>
        </aside>

        <!-- Content Area -->
        <main class="flex-1 overflow-y-auto bg-slate-50">
            <header class="bg-white p-4 shadow-sm flex justify-between items-center sticky top-0 z-10">
                <h2 class="font-bold text-slate-800 text-lg">Bem-vindo, <span id="store-name-display" class="text-blue-600">Loja</span></h2>
                <div class="flex items-center space-x-4">
                    <button onclick="openModal()" class="bg-blue-600 hover:bg-blue-700 text-white px-5 py-2.5 rounded-xl font-bold shadow-md flex items-center space-x-2 transition-all">
                        <i class="fas fa-plus"></i>
                        <span>Novo Produto</span>
                    </button>
                </div>
            </header>

            <div class="p-6 max-w-6xl mx-auto">
                <div id="products-list" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"></div>
                <div id="empty-state" class="hidden py-20 text-center">
                    <div class="bg-slate-200 w-20 h-20 rounded-full flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-box-open text-slate-400 text-3xl"></i>
                    </div>
                    <h3 class="text-lg font-bold text-slate-700">Nenhum produto ainda</h3>
                    <p class="text-slate-500">Clique em "Novo Produto" para começar a vender.</p>
                </div>
            </div>
        </main>
    </div>

    <!-- Modais omitidos para brevidade mas mantidos na lógica (Delete Account e Product Modal) -->
    <!-- ... (Delete Account Modal) ... -->
    <div id="delete-account-modal" class="hidden fixed inset-0 bg-black/60 z-50 flex items-center justify-center p-4 backdrop-blur-sm">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-2xl p-8 text-center">
            <div class="bg-red-100 w-20 h-20 rounded-full flex items-center justify-center mx-auto mb-6">
                <i class="fas fa-exclamation-triangle text-red-600 text-3xl"></i>
            </div>
            <h3 class="text-2xl font-bold text-slate-900 mb-2">Excluir sua Loja?</h3>
            <p class="text-slate-500 mb-8 leading-relaxed">Esta ação é <b>permanente</b>. Todos os seus produtos cadastrados serão removidos do marketplace.</p>
            <div class="flex flex-col space-y-3">
                <button id="btn-final-delete" class="w-full bg-red-600 hover:bg-red-700 text-white font-bold py-3 rounded-xl shadow-lg transition-all">Sim, Excluir Tudo</button>
                <button onclick="closeDeleteModal()" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-600 font-bold py-3 rounded-xl transition-all">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- Product Modal -->
    <div id="modal" class="hidden fixed inset-0 bg-black/50 z-40 flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-lg rounded-2xl shadow-2xl overflow-hidden">
            <div class="p-6 border-b flex justify-between items-center">
                <h3 id="modal-title" class="text-xl font-bold text-slate-900">Novo Produto</h3>
                <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600"><i class="fas fa-times text-xl"></i></button>
            </div>
            <form id="product-form" class="p-6 space-y-4">
                <input type="hidden" id="edit-id">
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">Nome do Produto</label>
                    <input type="text" id="p-name" required class="w-full px-4 py-2 rounded-lg border border-slate-200 focus:border-blue-500 outline-none">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Preço (R$)</label>
                        <input type="number" step="0.01" id="p-price" required class="w-full px-4 py-2 rounded-lg border border-slate-200 focus:border-blue-500 outline-none">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Categoria</label>
                        <select id="p-category" class="w-full px-4 py-2 rounded-lg border border-slate-200 focus:border-blue-500 outline-none">
                            <option value="Doces">Doces</option>
                            <option value="Salgados">Salgados</option>
                            <option value="Bebidas">Bebidas</option>
                            <option value="Artesanato">Artesanato</option>
                        </select>
                    </div>
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">URL da Imagem</label>
                    <input type="url" id="p-image" required class="w-full px-4 py-2 rounded-lg border border-slate-200 focus:border-blue-500 outline-none" placeholder="https://exemplo.com/foto.jpg">
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">Descrição</label>
                    <textarea id="p-desc" rows="3" class="w-full px-4 py-2 rounded-lg border border-slate-200 focus:border-blue-500 outline-none"></textarea>
                </div>
                <div class="pt-4 flex space-x-3">
                    <button type="button" onclick="closeModal()" class="flex-1 py-2 text-slate-600 font-medium border rounded-lg hover:bg-slate-50">Cancelar</button>
                    <button type="submit" class="flex-1 py-2 bg-blue-600 text-white font-bold rounded-lg hover:bg-blue-700 shadow-md">Guardar</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Firebase JS -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInAnonymously } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, setDoc, getDoc, addDoc, deleteDoc, onSnapshot, getDocs } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDXt1bezrUJcTNRKwmYFN6obE4PcZucnUE",
            authDomain: "schoolmarket-a6c1c.firebaseapp.com",
            projectId: "schoolmarket-a6c1c",
            storageBucket: "schoolmarket-a6c1c.firebasestorage.app",
            messagingSenderId: "800255721305",
            appId: "1:800255721305:web:cbefc7c385f19646501648"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const appId = 'schoolmarket-a6c1c';

        let sellerEmail = null;
        let sellerStoreName = "Minha Loja";
        let sellerWhatsApp = "";
        let isRegister = false;

        onAuthStateChanged(auth, async (user) => {
            if (user) {
                const saved = localStorage.getItem(`sm_session_${appId}`);
                if (saved) {
                    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'sellers', saved);
                    const snap = await getDoc(docRef);
                    if(snap.exists()) {
                        sellerEmail = saved;
                        sellerStoreName = snap.data().storeName;
                        sellerWhatsApp = snap.data().whatsapp || "";
                        showDashboard();
                    } else { showAuth(); }
                } else { showAuth(); }
            } else { await signInAnonymously(auth); }
            document.getElementById('loading-overlay').classList.add('hidden');
        });

        document.getElementById('auth-form').onsubmit = async (e) => {
            e.preventDefault();
            const email = document.getElementById('auth-email').value.toLowerCase().trim();
            const pass = document.getElementById('auth-password').value;
            const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'sellers', email);

            try {
                const snap = await getDoc(docRef);
                if (isRegister) {
                    const storeNameInput = document.getElementById('auth-store-name').value.trim();
                    const whatsappInput = document.getElementById('auth-whatsapp').value.trim().replace(/\D/g,'');
                    if (snap.exists()) return alert("E-mail já registado!");
                    if (!storeNameInput || !whatsappInput) return alert("Preencha todos os campos!");
                    
                    await setDoc(docRef, { 
                        password: pass, 
                        storeName: storeNameInput, 
                        whatsapp: whatsappInput,
                        createdAt: Date.now() 
                    });
                    enter(email, storeNameInput, whatsappInput);
                } else {
                    if (!snap.exists() || snap.data().password !== pass) return alert("Dados inválidos!");
                    enter(email, snap.data().storeName, snap.data().whatsapp);
                }
            } catch (err) { alert("Erro de conexão."); }
        };

        function enter(email, storeName, whatsapp) {
            sellerEmail = email;
            sellerStoreName = storeName;
            sellerWhatsApp = whatsapp;
            localStorage.setItem(`sm_session_${appId}`, email);
            showDashboard();
        }

        window.logout = () => {
            localStorage.removeItem(`sm_session_${appId}`);
            location.reload();
        };

        function showDashboard() {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('dashboard').classList.remove('hidden');
            document.getElementById('store-name-display').innerText = sellerStoreName;
            initProductsListener();
        }

        function showAuth() {
            document.getElementById('dashboard').classList.add('hidden');
            document.getElementById('auth-screen').classList.remove('hidden');
        }

        document.getElementById('toggle-auth').onclick = () => {
            isRegister = !isRegister;
            document.getElementById('register-fields').classList.toggle('hidden', !isRegister);
            document.getElementById('auth-title').innerText = isRegister ? "Criar Conta de Vendedor" : "Portal do Vendedor";
            document.getElementById('auth-submit-btn').innerText = isRegister ? "Criar Minha Loja" : "Entrar no Painel";
            document.getElementById('toggle-auth').innerText = isRegister ? "Já tenho conta? Entrar" : "Ainda não tem conta? Registar Loja";
        };

        function initProductsListener() {
            const q = collection(db, 'artifacts', appId, 'public', 'data', 'products');
            onSnapshot(q, (snap) => {
                const list = snap.docs.map(d => ({id: d.id, ...d.data()})).filter(p => p.ownerEmail === sellerEmail);
                render(list);
            });
        }

        function render(list) {
            const container = document.getElementById('products-list');
            const empty = document.getElementById('empty-state');
            container.innerHTML = '';
            if (list.length === 0) return empty.classList.remove('hidden');
            empty.classList.add('hidden');
            list.forEach(p => {
                container.innerHTML += `
                    <div class="bg-white rounded-xl shadow-sm border border-slate-100 overflow-hidden">
                        <img src="${p.image}" class="w-full h-40 object-cover bg-slate-100">
                        <div class="p-4">
                            <div class="flex justify-between items-start mb-2">
                                <h4 class="font-bold text-slate-800">${p.name}</h4>
                                <span class="text-blue-600 font-bold">R$ ${p.price.toFixed(2)}</span>
                            </div>
                            <div class="flex space-x-2 mt-4">
                                <button onclick="editItem('${p.id}')" class="flex-1 py-2 bg-slate-50 text-slate-600 rounded-lg text-sm font-bold border hover:bg-blue-50 hover:text-blue-600 transition-colors">EDITAR</button>
                                <button onclick="deleteItem('${p.id}')" class="px-3 py-2 text-slate-300 hover:text-red-500"><i class="fas fa-trash-alt"></i></button>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        document.getElementById('product-form').onsubmit = async (e) => {
            e.preventDefault();
            const id = document.getElementById('edit-id').value;
            const data = {
                name: document.getElementById('p-name').value,
                price: parseFloat(document.getElementById('p-price').value),
                category: document.getElementById('p-category').value,
                image: document.getElementById('p-image').value,
                description: document.getElementById('p-desc').value,
                ownerEmail: sellerEmail,
                storeName: sellerStoreName,
                whatsapp: sellerWhatsApp, // Salva o WhatsApp atual da loja no produto
                updatedAt: Date.now()
            };
            const col = collection(db, 'artifacts', appId, 'public', 'data', 'products');
            if (id) await setDoc(doc(col, id), data, { merge: true });
            else await addDoc(col, data);
            closeModal();
        };

        window.deleteItem = async (id) => { if (confirm("Remover produto?")) await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id)); };
        window.editItem = async (id) => {
            const d = await getDoc(doc(db, 'artifacts', appId, 'public', 'data', 'products', id));
            if (!d.exists()) return;
            const p = d.data();
            document.getElementById('modal-title').innerText = "Editar Produto";
            document.getElementById('edit-id').value = id;
            document.getElementById('p-name').value = p.name;
            document.getElementById('p-price').value = p.price;
            document.getElementById('p-category').value = p.category;
            document.getElementById('p-image').value = p.image;
            document.getElementById('p-desc').value = p.description;
            document.getElementById('modal').classList.remove('hidden');
        };
        window.openModal = () => {
            document.getElementById('modal-title').innerText = "Novo Produto";
            document.getElementById('edit-id').value = "";
            document.getElementById('product-form').reset();
            document.getElementById('modal').classList.remove('hidden');
        };
        window.closeModal = () => document.getElementById('modal').classList.add('hidden');
        window.closeDeleteModal = () => document.getElementById('delete-account-modal').classList.add('hidden');
        window.confirmDeleteAccount = () => document.getElementById('delete-account-modal').classList.remove('hidden');
        document.getElementById('btn-final-delete').onclick = async () => {
            const productsCol = collection(db, 'artifacts', appId, 'public', 'data', 'products');
            const productsSnap = await getDocs(productsCol);
            productsSnap.forEach(async (d) => { if(d.data().ownerEmail === sellerEmail) await deleteDoc(d.ref); });
            await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'sellers', sellerEmail));
            logout();
        };
    </script>
</body>
</html>
