# -soufian07072
my-first-code
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Soufiane Market | منصة البيع والشراء</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Tajawal', sans-serif; }
        .messenger-shadow { box-shadow: 0 -5px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); }
        .chat-animate { transition: all 0.3s ease-in-out; transform: translateY(100%); opacity: 0; }
        .chat-animate.open { transform: translateY(0); opacity: 1; }
    </style>
</head>
<body class="bg-gray-50 pb-20">

    <nav class="bg-white border-b sticky top-0 z-50">
        <div class="container mx-auto px-4 h-16 flex items-center justify-between">
            <div class="flex items-center gap-2">
                <div class="w-10 h-10 bg-indigo-600 rounded-lg flex items-center justify-center text-white">
                    <i class="fas fa-store text-xl"></i>
                </div>
                <h1 class="text-xl font-bold text-gray-800">سفيان ماركت</h1>
            </div>
            <button onclick="toggleAddProduct()" class="bg-indigo-600 text-white px-4 py-2 rounded-full text-sm font-bold hover:bg-indigo-700 transition">
                + إضافة منتج
            </button>
        </div>
    </nav>

    <main class="container mx-auto px-4 py-8">
        <div class="flex items-center justify-between mb-8">
            <h2 class="text-2xl font-bold text-gray-800">اكتشف المنتجات</h2>
            <span class="bg-green-100 text-green-700 text-xs font-bold px-3 py-1 rounded-full">رسوم المنصة: 10% فقط</span>
        </div>

        <div id="products-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
            </div>
    </main>

    <div id="chat-window" class="chat-animate fixed bottom-4 left-4 w-80 md:w-96 bg-white rounded-2xl border messenger-shadow z-50 flex flex-col overflow-hidden">
        <div class="bg-indigo-600 p-4 text-white flex justify-between items-center">
            <div class="flex items-center gap-3">
                <div class="w-8 h-8 bg-white/20 rounded-full flex items-center justify-center">
                    <i class="fas fa-user text-sm"></i>
                </div>
                <div>
                    <p id="chat-user-name" class="font-bold text-sm leading-none">اسم البائع</p>
                    <span class="text-[10px] opacity-70">متصل الآن</span>
                </div>
            </div>
            <button onclick="closeChat()" class="hover:bg-white/10 w-8 h-8 rounded-full">✕</button>
        </div>

        <div id="chat-body" class="h-80 overflow-y-auto p-4 flex flex-col gap-3 bg-gray-50">
            </div>

        <div class="p-4 border-t bg-white">
            <div class="flex gap-2">
                <input type="text" id="chat-input" placeholder="اكتب رسالة للبائع..." class="flex-1 bg-gray-100 border-none rounded-full px-4 py-2 text-sm focus:ring-2 focus:ring-indigo-500 outline-none">
                <button onclick="sendMsg()" class="bg-indigo-600 text-white w-10 h-10 rounded-full flex items-center justify-center hover:bg-indigo-700">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
        </div>
    </div>

    <script>
        const COMMISSION_RATE = 0.10; // عمولتك 10%

        // قاعدة بيانات تجريبية (يتم ربطها بـ Firebase لاحقاً)
        const products = [
            { id: 1, name: "سترة شتوية عصرية", seller: "محمد أمين", basePrice: 300, img: "https://images.unsplash.com/photo-1591047139829-d91aecb6caea?w=400" },
            { id: 2, name: "ساعة كلاسيكية", seller: "ليلى", basePrice: 150, img: "https://images.unsplash.com/photo-1524592094714-0f0654e20314?w=400" },
            { id: 3, name: "حذاء رياضي", seller: "حمزة", basePrice: 450, img: "https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=400" },
            { id: 4, name: "نظارات شمسية", seller: "يوسف", basePrice: 120, img: "https://images.unsplash.com/photo-1572635196237-14b3f281503f?w=400" }
        ];

        // عرض المنتجات مع حساب العمولة
        function renderProducts() {
            const grid = document.getElementById('products-grid');
            grid.innerHTML = products.map(product => {
                const totalPrice = Math.round(product.basePrice * (1 + COMMISSION_RATE));
                return `
                    <div class="bg-white rounded-2xl border border-gray-100 overflow-hidden hover:shadow-xl transition-shadow group">
                        <div class="relative overflow-hidden">
                            <img src="${product.img}" class="w-full h-56 object-cover group-hover:scale-110 transition-transform duration-500">
                            <div class="absolute top-2 right-2 bg-white/90 backdrop-blur px-2 py-1 rounded-lg text-[10px] font-bold shadow-sm">
                                بائع موثوق
                            </div>
                        </div>
                        <div class="p-4">
                            <h3 class="font-bold text-gray-800 mb-1">${product.name}</h3>
                            <p class="text-xs text-gray-500 mb-3 flex items-center gap-1">
                                <i class="fas fa-user-circle"></i> ${product.seller}
                            </p>
                            <div class="flex items-center justify-between border-t pt-3">
                                <div>
                                    <span class="block text-[10px] text-gray-400">السعر النهائي</span>
                                    <span class="text-lg font-bold text-indigo-600">${totalPrice} درهم</span>
                                </div>
                                <button onclick="openChat('${product.seller}', '${product.name}')" class="bg-gray-100 text-gray-800 p-2 rounded-xl hover:bg-indigo-600 hover:text-white transition">
                                    <i class="fab fa-facebook-messenger text-xl"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // نظام الشات (Messenger Logic)
        const chatWin = document.getElementById('chat-window');
        const chatBody = document.getElementById('chat-body');
        const chatInput = document.getElementById('chat-input');

        function openChat(seller, item) {
            document.getElementById('chat-user-name').innerText = seller;
            chatWin.classList.add('open');
            chatBody.innerHTML = `
                <div class="bg-indigo-50 text-indigo-700 p-3 rounded-lg text-[12px] text-center mb-2">
                    أنت الآن تراسل <b>${seller}</b> بخصوص <b>${item}</b>. <br> تذكر دائماً الدفع عبر المنصة لضمان حقك.
                </div>
            `;
        }

        function closeChat() {
            chatWin.classList.remove('open');
        }

        function sendMsg() {
            const val = chatInput.value.trim();
            if(!val) return;

            // إضافة رسالة المشتري
            chatBody.innerHTML += `
                <div class="bg-indigo-600 text-white p-3 rounded-2xl rounded-bl-none self-end max-w-[80%] text-sm shadow-sm">
                    ${val}
                </div>
            `;
            chatInput.value = '';
            chatBody.scrollTop = chatBody.scrollHeight;

            // رد تلقائي بسيط للمحاكاة
            setTimeout(() => {
                chatBody.innerHTML += `
                    <div class="bg-white border p-3 rounded-2xl rounded-br-none self-start max-w-[80%] text-sm shadow-sm text-gray-700">
                        مرحباً! نعم المنتج لا يزال متوفراً.
                    </div>
                `;
                chatBody.scrollTop = chatBody.scrollHeight;
            }, 1000);
        }

        // تشغيل العرض عند التحميل
        renderProducts();

        // دعم الإرسال عبر مفتاح Enter
        chatInput.addEventListener('keypress', (e) => { if(e.key === 'Enter') sendMsg(); });

    </script>
</body>
</html>
