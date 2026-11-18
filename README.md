<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#265d26">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Kisan Hisab">
    <title>Kisan Kheti Hisab — Mobile App</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter & Hind for crisp Hindi-English typography -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Hind:wght@400;500;600;700&family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Font Awesome for clean agricultural icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- Leaflet CSS & JS for Map Area Measurement -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'Hind', 'sans-serif'],
                    },
                    colors: {
                        kisan: {
                            50: '#f2f9f1',
                            100: '#e1f2df',
                            200: '#c4e4c1',
                            300: '#98ce95',
                            400: '#65b162',
                            500: '#3f933d',
                            600: '#2e762d',
                            700: '#265d26',
                            800: '#214b21',
                            900: '#1c3e1d',
                        },
                        gold: {
                            50: '#fefce8',
                            100: '#fef9c3',
                            400: '#facc15',
                            500: '#eab308',
                            600: '#ca8a04',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #eef3eb;
            font-family: 'Inter', 'Hind', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        @media (min-width: 768px) {
            body {
                background: radial-gradient(circle at top, #dce8d6 0%, #c8d9c0 100%);
                padding-top: 1.5rem;
                padding-bottom: 2rem;
            }
            #app-shell {
                max-width: 480px;
                margin: 0 auto;
                min-height: 860px;
                border-radius: 2.25rem;
                box-shadow: 0 25px 60px -15px rgba(22, 48, 22, 0.35), 0 0 0 8px #1c3e1d;
                overflow: hidden;
                position: relative;
                background-color: #f4f7f2;
            }
        }
        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        input[type="number"] {
            -moz-appearance: textfield;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            body {
                background-color: white !important;
                padding: 0 !important;
            }
            #app-shell {
                max-width: 100% !important;
                box-shadow: none !important;
                border-radius: 0 !important;
            }
            .tab-panel {
                display: block !important;
            }
        }
    </style>
</head>
<body class="text-slate-800 antialiased">

    <!-- Language Selection & Splash Screen Intro -->
    <div id="splash-screen" class="fixed inset-0 z-50 bg-gradient-to-br from-kisan-900 via-kisan-800 to-kisan-950 text-white flex flex-col items-center justify-center p-6 transition-opacity duration-700">
        <div class="bg-gold-500 text-kisan-950 w-24 h-24 rounded-3xl shadow-2xl flex items-center justify-center border-4 border-white/20 mb-6 animate-bounce">
            <i class="fa-solid fa-wheat-awn text-5xl"></i>
        </div>
        <h1 class="text-2xl sm:text-3xl font-black tracking-tight text-center mb-1">
            Kisan Kheti Hisab
        </h1>
        <p class="text-gold-300 text-xs font-bold tracking-wider uppercase mb-6">Created by BK — Kisan Kheti Hisab</p>
        
        <!-- Language Picker Box -->
        <div class="bg-white/10 backdrop-blur-md p-4 rounded-2xl w-full max-w-xs border border-white/20 mb-6 space-y-2">
            <label class="block text-xs font-bold text-gold-300 text-center mb-2">Apni Bhasha Chunein / Select Language:</label>
            <div class="grid grid-cols-3 gap-2">
                <button type="button" onclick="setAppLanguage('hi')" id="lang-btn-hi" class="py-2.5 px-2 rounded-xl text-xs font-black bg-gold-500 text-kisan-950 shadow transition">हिंदी (Hindi)</button>
                <button type="button" onclick="setAppLanguage('en')" id="lang-btn-en" class="py-2.5 px-2 rounded-xl text-xs font-bold bg-white/20 text-white hover:bg-white/30 transition">English</button>
                <button type="button" onclick="setAppLanguage('cg')" id="lang-btn-cg" class="py-2.5 px-2 rounded-xl text-xs font-bold bg-white/20 text-white hover:bg-white/30 transition">छत्तीसगढ़ी</button>
            </div>
        </div>

        <button type="button" onclick="closeSplashScreen()" class="bg-gold-500 hover:bg-gold-400 text-kisan-950 font-black text-sm px-8 py-3.5 rounded-2xl shadow-xl active:scale-95 transition flex items-center gap-2">
            <span id="splash-btn-text">App Shuru Karein</span>
            <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>

    <!-- Toast Notification Banner -->
    <div id="toast-container" class="fixed top-20 left-1/2 -translate-x-1/2 z-50 flex flex-col gap-2 pointer-events-none w-[90%] max-w-sm"></div>

    <!-- Mobile App Shell Container -->
    <div id="app-shell" class="min-h-screen flex flex-col bg-[#f4f7f2] pb-24 md:pb-24">

        <!-- App Top Bar (Header) -->
        <header class="bg-gradient-to-r from-kisan-800 via-kisan-700 to-kisan-600 text-white shadow-md sticky top-0 z-30">
            <div class="px-4 py-3.5">
                <div class="flex items-center justify-between gap-2">
                    <div class="flex items-center gap-2.5">
                        <div class="bg-gold-500 text-kisan-900 w-11 h-11 rounded-2xl shadow-md flex items-center justify-center shrink-0 border-2 border-white/20">
                            <i class="fa-solid fa-wheat-awn text-2xl text-kisan-950"></i>
                        </div>
                        <div>
                            <h1 class="text-base sm:text-lg font-extrabold tracking-tight leading-tight flex items-center gap-1.5">
                                Kisan Kheti Hisab
                                <span class="text-[10px] font-bold bg-gold-400 text-kisan-950 px-2 py-0.5 rounded-full">BK APP</span>
                            </h1>
                            <p class="text-kisan-100 text-[11px] font-medium leading-tight">Created by BK — Kisan Kheti Hisab</p>
                        </div>
                    </div>

                    <!-- Top Quick Actions -->
                    <div class="flex items-center gap-1.5 no-print">
                        <button type="button" onclick="fillDemoData()" title="Demo Data Bharein" class="inline-flex items-center justify-center gap-1 bg-gold-500 hover:bg-gold-400 text-kisan-950 font-bold text-xs px-2.5 py-2 rounded-xl shadow active:scale-95 transition">
                            <i class="fa-solid fa-wand-magic-sparkles"></i>
                            <span>Demo</span>
                        </button>
                        <button type="button" onclick="resetForm()" title="Reset Form" class="inline-flex items-center justify-center bg-white/15 hover:bg-white/25 text-white text-xs p-2 rounded-xl active:scale-95 transition">
                            <i class="fa-solid fa-rotate-right"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Live Sticky Mini Summary Strip -->
            <div class="bg-kisan-900/60 backdrop-blur-sm px-4 py-2 flex items-center justify-between text-xs border-t border-white/10">
                <div>
                    <span class="text-kisan-200">Lagat:</span>
                    <span id="mini-bar-cost" class="font-bold text-white ml-1">₹ 0</span>
                </div>
                <div>
                    <span class="text-kisan-200">Bikri:</span>
                    <span id="mini-bar-rev" class="font-bold text-emerald-300 ml-1">₹ 0</span>
                </div>
                <button type="button" onclick="switchTab('result')" class="flex items-center gap-1 bg-white/15 px-2.5 py-0.5 rounded-full font-bold">
                    <span id="mini-bar-net" class="text-gold-300">₹ 0</span>
                    <i class="fa-solid fa-chevron-right text-[10px]"></i>
                </button>
            </div>
        </header>

        <!-- Main App Content Area (Tabbed Screens) -->
        <main class="flex-1 px-3.5 pt-4 space-y-4">

            <!-- Horizontal Quick-Select Crop Icons Carousel (Moved to Very Top) -->
            <section class="bg-white rounded-2xl p-3 shadow-sm border border-kisan-100">
                <div class="flex items-center justify-between mb-2 px-1">
                    <span class="text-xs font-bold text-kisan-800 flex items-center gap-1.5">
                        <i class="fa-solid fa-bolt text-gold-500"></i> Fasal Turant Chunein (Quick Select)
                    </span>
                    <span class="text-[10px] text-slate-400">Scroll karein 👉</span>
                </div>
                <div class="flex items-center gap-2.5 overflow-x-auto pb-1.5 scrollbar-none" style="-webkit-overflow-scrolling: touch;">
                    <button type="button" onclick="quickSelectCrop('Gehu (Wheat)', 'Grains')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-wheat-awn text-kisan-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Gehu</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Dhaan (Paddy/Rice)', 'Grains')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-kisan-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Dhaan</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Aalu (Potato)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-carrot text-amber-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Aalu</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Tamatar (Tomato)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-apple-whole text-rose-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Tamatar</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Apple (Seb)', 'Fruits')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-apple-whole text-red-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Apple</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Ganna (Sugarcane)', 'OilseedsCash')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-cubes text-emerald-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Ganna</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Sarso (Mustard)', 'OilseedsCash')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-yellow-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Sarso</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Pyaj (Onion)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-circle text-purple-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Pyaj</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Mirch (Chilli)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-pepper-hot text-red-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Mirch</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Tarbooj (Watermelon)', 'Fruits')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-circle-dot text-emerald-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Tarbooj</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Kheera (Cucumber)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-emerald-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Kheera</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Loki (Bottle Gourd)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-green-700 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Loki</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Lahsun (Garlic)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-amber-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Lahsun</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Matar (Peas)', 'Vegetables')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-emerald-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Matar</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Angoor (Grapes)', 'Fruits')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-purple-600 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Angoor</span>
                    </button>
                    <button type="button" onclick="quickSelectCrop('Litchi', 'Fruits')" class="flex flex-col items-center justify-center p-2 rounded-xl bg-kisan-50 hover:bg-kisan-100 border border-kisan-200 shrink-0 w-16 transition active:scale-95">
                        <i class="fa-solid fa-seedling text-rose-500 text-lg mb-1"></i>
                        <span class="text-[10px] font-bold text-slate-700 truncate w-full text-center">Litchi</span>
                    </button>
                </div>
            </section>

            <!-- Kisan Lucky Dhan & Govt Yojana Reward Banner -->
            <section class="bg-gradient-to-r from-amber-600 via-orange-600 to-amber-700 text-white rounded-2xl p-4 shadow-sm relative overflow-hidden">
                    <div class="flex items-center justify-between relative z-10">
                        <div class="space-y-1">
                            <span class="bg-gold-400 text-kisan-950 text-[10px] font-extrabold px-2.5 py-0.5 rounded-full uppercase"><i class="fa-solid fa-gift mr-1"></i>KISAN REWARD & YOGANA</span>
                            <h2 class="text-base font-extrabold">Har Mahine 10,000 tak ka Inam & PM-Kisan Update</h2>
                            <p class="text-xs text-amber-100">App share karein aur sarkari yojanaon (Subsidy) ka seedha labh paayein!</p>
                        </div>
                        <button type="button" onclick="shareAppWithFriends()" class="bg-white text-amber-900 font-extrabold text-xs px-3.5 py-2.5 rounded-xl shadow active:scale-95 transition shrink-0 flex items-center gap-1.5">
                            <i class="fa-solid fa-share-nodes text-sm"></i>
                            <span>App Share Karein</span>
                        </button>
                    </div>
                </section>

                <!-- Live Weather & Spray Alert Banner Card -->
                <section class="bg-gradient-to-r from-sky-700 to-blue-800 text-white rounded-2xl p-4 shadow-sm relative overf
