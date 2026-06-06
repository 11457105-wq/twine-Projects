<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>龍潭大池埤塘數位資料庫</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <style>
        /* ==================== 核心全域樣式（網頁暗色系美化） ==================== */
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: "Helvetica Neue", Arial, "Noto Sans TC", sans-serif; }
        body { background-color: #0f172a; color: #f1f5f9; line-height: 1.6; }
        a { text-decoration: none; color: inherit; }

        /* 頁首 */
        .main-header { background-color: #1e293b; padding: 20px; text-align: center; border-bottom: 3px solid #0284c7; }
        .main-header .logo { font-size: 1.6rem; font-weight: bold; color: #38bdf8; }

        /* 導覽列與下拉選單 */
        .main-nav { background-color: #1e293b; border-bottom: 1px solid #334155; position: sticky; top: 0; z-index: 100; }
        .nav-list { display: flex; justify-content: center; list-style: none; flex-wrap: wrap; }
        .nav-list > li { position: relative; }
        .nav-list > li > a { display: block; padding: 15px 20px; font-weight: 500; color: #94a3b8; transition: all 0.3s; }
        .nav-list > li > a:hover { color: #38bdf8; background-color: #0f172a; }
        
        .dropdown:hover .dropdown-menu { display: block; }
        .dropdown-menu { display: none; position: absolute; top: 100%; left: 0; background-color: #1e293b; list-style: none; min-width: 200px; border: 1px solid #334155; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.5); }
        .dropdown-menu a { display: block; padding: 12px 16px; color: #94a3b8; font-size: 0.95rem; }
        .dropdown-menu a:hover { background-color: #0f172a; color: #38bdf8; }

        /* 主要內容分頁控制 */
        .main-content { max-width: 1200px; margin: 30px auto; padding: 0 20px; min-height: 70vh; }
        .page-section { display: none; } /* 預設全部隱藏 */
        .page-section.active { display: block; animation: fadeIn 0.4s ease-in-out; } /* 只顯示被啟動的分頁 */

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 看板與卡片樣式 */
        .welcome-box { background-color: #1e293b; padding: 30px; border-radius: 12px; border: 1px solid #334155; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3); margin-bottom: 25px; }
        .welcome-img { width: 100%; max-height: 400px; object-fit: cover; border-radius: 8px; margin: 20px 0; border: 1px solid #475569; }
        .text-card { border-left: 5px solid #38bdf8; }
        .author { float: right; font-size: 0.9rem; color: #64748b; background-color: #0f172a; padding: 2px 8px; border-radius: 4px; }
        .article-p { margin-bottom: 15px; color: #cbd5e1; font-size: 1.05rem; text-align: justify; }
        
        /* 生態卡片網格 */
        .section-title-box { margin-bottom: 25px; border-bottom: 2px solid #334155; padding-bottom: 10px; }
        .section-title-box h2 { color: #38bdf8; }
        .card-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 25px; margin-top: 20px; }
        .bird-card { background-color: #1e293b; border-radius: 12px; overflow: hidden; border: 1px solid #334155; transition: transform 0.3s; display: flex; flex-direction: column; }
        .bird-card:hover { transform: translateY(-5px); box-shadow: 0 10px 20px rgba(0,0,0,0.5); }
        .thumb-wrapper { width: 100%; height: 200px; background-color: #0f172a; overflow: hidden; }
        .thumb-wrapper img { width: 100%; height: 100%; object-fit: cover; }
        .card-body { padding: 20px; flex-grow: 1; display: flex; flex-direction: column; justify-content: space-between; }
        .card-body h3 { color: #f8fafc; margin-bottom: 10px; font-size: 1.25rem; }
        .card-text { color: #94a3b8; font-size: 0.95rem; margin-bottom: 15px; text-align: justify; }
        .cta-btn { background-color: #0284c7; color: white; border: none; padding: 10px 15px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; text-align: center; transition: background 0.2s; }
        .cta-btn:hover { background-color: #0369a1; }

        /* 水質與昆蟲專屬風格美化 */
        .water-grid { display: grid; grid-template-columns: 1fr; gap: 20px; margin-bottom: 30px; }
        @media (min-width: 768px) { .water-grid { grid-template-columns: 1fr 1fr; } }
        .water-card { background-color: #1e293b; border: 1px solid #334155; border-radius: 12px; padding: 24px; }
        .water-card h3 { color: #38bdf8; font-size: 1.2rem; margin-bottom: 16px; border-bottom: 1px solid #334155; padding-bottom: 8px; }
        .water-list { list-style: none; }
        .water-list li { font-size: 0.95rem; color: #cbd5e1; margin-bottom: 14px; }
        .water-list li strong { color: #38bdf8; font-size: 1rem; }

        /* 地圖、表格與問答互動樣式 */
        .map-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; border-radius: 8px; border: 1px solid #334155; }
        .map-container iframe { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
        .struc-table { width: 100%; border-collapse: collapse; margin-top: 15px; background-color: #1e293b; }
        .struc-table th, .struc-table td { border: 1px solid #334155; padding: 12px; text-align: left; }
        .struc-table th { background-color: #0f172a; color: #38bdf8; }
        
        /* 融合暗色系互動任務設計 */
        .quiz-item { background-color: #0f172a; padding: 18px; border-radius: 8px; margin-bottom: 20px; border: 1px solid #334155; }
        .quiz-title { font-weight: bold; font-size: 1.05rem; margin-bottom: 12px; color: #f1f5f9; }
        
        .option-btn { display: block; width: 100%; text-align: left; background: #1e293b; border: 1px solid #475569; color: #cbd5e1; padding: 12px 16px; margin-bottom: 8px; border-radius: 6px; cursor: pointer; font-size: 0.95rem; transition: all 0.2s ease; }
        .option-btn:hover { background: #334155; border-color: #38bdf8; color: #fff; }
        .option-btn.correct { background-color: #065f46; border-color: #10b981; color: #ecfdf5; font-weight: bold; }
        .option-btn.wrong { background-color: #991b1b; border-color: #ef4444; color: #fef2f2; }
        
        .analysis-box { margin-top: 10px; padding: 12px; background-color: #0c4a6e; border-left: 4px solid #0284c7; border-radius: 4px; display: none; font-size: 0.9rem; color: #e0f2fe; }
        
        .fill-blank-input { border: none; border-bottom: 2px solid #38bdf8; padding: 2px 6px; font-size: 1rem; width: 100px; text-align: center; outline: none; background: transparent; color: #38bdf8; font-weight: bold; }
        .check-btn { background-color: #0284c7; color: white; border: none; padding: 8px 16px; border-radius: 4px; cursor: pointer; font-size: 0.9rem; margin-top: 10px; font-weight: bold; transition: background 0.2s; }
        .check-btn:hover { background-color: #0369a1; }
        .result-text { margin-left: 15px; font-weight: bold; font-size: 1rem; }
        
        .text-area-answer { width: 100%; height: 80px; background-color: #0f172a; border: 1px solid #475569; border-radius: 6px; padding: 10px; color: #f1f5f9; margin-top: 8px; box-sizing: border-box; resize: vertical; font-family: inherit; }
        .reference-answer { margin-top: 10px; background: #7c2d12; padding: 12px; border-radius: 5px; border-left: 4px solid #ea580c; display: none; font-size: 0.9rem; color: #ffedd5; }

        /* 暗色系流程圖樣式 */
        .flowchart { background: #0f172a; padding: 20px; border-radius: 8px; text-align: center; font-weight: bold; margin: 20px 0; border: 1px solid #334155; display: flex; flex-wrap: wrap; justify-content: center; align-items: center; gap: 8px; }
        .flow-step { background: #1e293b; padding: 8px 14px; border-radius: 6px; border: 1px solid #475569; font-size: 0.9rem; color: #38bdf8; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.2); }
        .arrow { color: #64748b; font-size: 1.1rem; }

        /* 原始結構與內建 details 樣式保留 */
        details { margin-top: 10px; background-color: #1e293b; padding: 10px; border-radius: 4px; border: 1px solid #475569; }
        summary { cursor: pointer; color: #38bdf8; font-weight: bold; }

        /* 彈出視窗 Modal */
        .custom-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.8); z-index: 1000; justify-content: center; align-items: center; padding: 20px; }
        .modal-content { background-color: #1e293b; padding: 30px; border-radius: 12px; max-width: 600px; width: 100%; border: 1px solid #475569; position: relative; color: #e2e8f0; }
        .modal-content h2 { color: #38bdf8; margin-bottom: 10px; }
        .modal-content hr { border: 0; border-top: 1px solid #334155; margin-bottom: 15px; }
        .modal-content p { margin-bottom: 12px; }
        .close-btn { position: absolute; top: 15px; right: 20px; font-size: 2rem; color: #94a3b8; cursor: pointer; }
        .close-btn:hover { color: #f1f5f9; }

        /* 頁尾 */
        .main-footer { background-color: #1e293b; text-align: center; padding: 20px; border-top: 1px solid #334155; color: #64748b; font-size: 0.9rem; margin-top: 50px; }
    </style>
</head>
<body>

    <header class="main-header">
        <div class="logo">
            <i class="fa-solid fa-water"></i> 龍潭大池埤塘數位資料庫
        </div>
    </header>

    <nav class="main-nav">
        <ul class="nav-list">
            <li><a href="javascript:void(0)" onclick="showPage('welcome')">關於</a></li>
            <li><a href="javascript:void(0)" onclick="showPage('map-page')">地圖</a></li>
            
            <li class="dropdown">
                <a href="javascript:void(0)">歷史與文化 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('hist-origin')">起源故事 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('hist-life')">周邊生活 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('hist-name')">地名考證 (劉亞蓁)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">生態調查 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('eco-plant')">植物 (張佳琪)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('eco-animal')">動物 (張佳琪)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('eco-bird')">鳥類 (張佳琪)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">昆蟲與水質 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('eco-insect')">昆蟲 (張佳琪)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('eco-water')">水質 (張佳琪)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">地景與水利 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('water-sys')">地形與水源 (李芷嫻)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('water-struc')">關鍵構造 (李芷嫻)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('water-logic')">分水邏輯 (李芷嫻)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">變遷與現狀 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('now-land')">土地變化 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('now-func')">轉型功能 (劉亞蓁)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('now-issue')">面臨挑戰 (劉亞蓁)</a></li>
                </ul>
            </li>

            <li class="dropdown">
                <a href="javascript:void(0)">互動任務 <i class="fa-solid fa-chevron-down"></i></a>
                <ul class="dropdown-menu">
                    <li><a href="javascript:void(0)" onclick="showPage('task-choice')">環境選擇題 (李芷嫻)</a></li>
                    <li><a href="javascript:void(0)" onclick="showPage('task-fill')">填空與思考題 (李芷嫻)</a></li>
                </ul>
            </li>
        </ul>
    </nav>

    <main class="main-content">

        <section id="welcome" class="page-section active">
            <div class="welcome-box">
                <h2>關於系統：歡迎來到龍潭大池數位資料庫</h2>
                <img src="data:image/jpeg;base64,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"alt="大池夜景">

                <p>本資料庫整合了龍潭大池的地景水利、歷史文化變遷以及豐富的埤塘生態調查資料。請點擊上方選單瀏覽各個專題頁面。</p>
            </div>
        </section>

        <section id="map-page" class="page-section">
            <div class="welcome-box">
                <h2>大池地理位置</h2>
                <div class="map-container">
                    <iframe src="https://maps.google.com/maps?q=龍潭大池&t=&z=15&ie=UTF8&iwloc=&output=embed" style="border:0;" allowfullscreen="" loading="lazy"></iframe>
                </div>
                <p style="margin-top: 15px; color: #8a99a6;"><i class="fa-solid fa-location-dot"></i> 地理標誌：桃園市龍潭區核心生態埤塘。</p>
            </div>
        </section>

        <section id="eco-plant" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：植物 (張佳琪)</h2>
                <p>探索棲息於龍潭大池的六大核心水生與挺水指標植物（點擊按鈕查看詳細紀錄）</p>
            </div>
            <div class="card-grid">
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/7/7a/Yellowwaterlili.jpg" alt="台灣萍蓬草"></div>
                    <div class="card-body">
                        <h3>台灣萍蓬草</h3>
                        <p class="card-text">台灣特有種的浮葉植物，也是大池生態復育的重點指標。全年開黃色小花，極具保育價值。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-1')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Typha-Orientalis.jpg/960px-Typha-Orientalis.jpg" alt="香蒲"></div>
                    <div class="card-body">
                        <h3>香蒲</h3>
                        <p class="card-text">常見的多年生挺水植物，因其花序形似蠟燭，又俗稱「水蠟燭」，能有效淨化埤塘水質。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-2')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Starr_061223-2773_Cyperus_involucratus.jpg/250px-Starr_061223-2773_Cyperus_involucratus.jpg" alt="風車草"></div>
                    <div class="card-body">
                        <h3>風車草</h3>
                        <p class="card-text">又稱輪傘草，莖頂叢生線形苞片如小風車。生命力極強，是護岸與挺水綠帶的主力植栽。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-3')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/36/Fleur_de_lotus.jpg/250px-Fleur_de_lotus.jpg" alt="蓮"></div>
                    <div class="card-body">
                        <h3>蓮</h3>
                        <p class="card-text">著名的挺水植物，夏季盛開。大池部分水域與人工景觀池有栽培，具備極高的觀賞與生態價值。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-4')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Vaginalis.jpg/250px-Vaginalis.jpg" alt="鴨舌草"></div>
                    <div class="card-body">
                        <h3>鴨舌草</h3>
                        <p class="card-text">埤塘常見的原始挺水植物，葉片先端漸尖形似鴨舌。開藍紫色小花，常見於岸邊濕地。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-5')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Eichhornia_crassipes_201510.jpg/250px-Eichhornia_crassipes_201510.jpg" alt="布袋蓮"></div>
                    <div class="card-body">
                        <h3>布袋蓮</h3>
                        <p class="card-text">侵略性強的漂浮植物。過度繁殖會覆蓋大池水面、阻絕陽光，是水利維護需定期清除的物種。</p>
                        <button class="cta-btn" onclick="openModal('modal-plant-6')">詳細植物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
            </div>
        </section>

        <section id="eco-animal" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：動物 (張佳琪)</h2>
                <p>探索棲息於龍潭大池周邊、陸域草叢與兩棲水域的指標性動物</p>
            </div>
            <div class="card-grid">
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Ocadia_sinensis.jpg/250px-Ocadia_sinensis.jpg" alt="斑龜"></div>
                    <div class="card-body">
                        <h3>斑龜</h3>
                        <p class="card-text">台灣最常見的本土原生爬行動物，頭部與頸部有顯眼的黃綠色細線條。常在晴天時爬上大池的木棧道或石塊上曬太陽。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-1')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/33/Takydromus_stejnegeri_166585190.jpg/250px-Takydromus_stejnegeri_166585190.jpg" alt="蓬萊草蜥"></div>
                    <div class="card-body">
                        <h3>蓬萊草蜥</h3>
                        <p class="card-text">台灣特有種蜥蜴，身體細長，尾巴極長。常出沒在大池周邊未硬化的草叢與低矮灌木叢中，主要捕食小型昆蟲。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-2')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fb/Rana_guentheri.jpg/250px-Rana_guentheri.jpg" alt="貢德氏赤蛙"></div>
                    <div class="card-body">
                        <h3>貢德氏赤蛙</h3>
                        <p class="card-text">大型兩棲類，叫聲如同狗吠「狂、狂」，極具辨識度。在大池周邊的挺水植物根部與濕軟泥土中非常活躍。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-3')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Procambarus_clarkii_top.jpg/250px-Procambarus_clarkii_top.jpg" alt="克氏原螯蝦"></div>
                    <div class="card-body">
                        <h3>克氏原螯蝦</h3>
                        <p class="card-text">惡名昭彰的外來種，雙螯強壯呈鮮紅色。牠們會在大池土堤邊掘洞，破壞護岸結構，並嚴重剪食本土水生植物的嫩芽。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-4')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Duttaphrynus_melanostictus_at_Niaosong_Wetland_Park%2C_Kaohsiung_2022-01-23_%28cropped%29.jpg/250px-Duttaphrynus_melanostictus_at_Niaosong_Wetland_Park%2C_Kaohsiung_2022-01-23_%28cropped%29.jpg" alt="黑眶蟾蜍"></div>
                    <div class="card-body">
                        <h3>黑眶蟾蜍</h3>
                        <p class="card-text">眼眶周圍有一圈明顯的黑色角質硬眶，皮膚佈滿癩蛤蟆疙瘩。多在夜間於環池步道、路燈下的草叢區覓食小型昆蟲。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-5')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Ornate_Pygmy_Frog_%28Microhyla_fissipes%29.jpg/250px-Ornate_Pygmy_Frog_%28Microhyla_fissipes%29.jpg" alt="小雨蛙"></div>
                    <div class="card-body">
                        <h3>小雨蛙</h3>
                        <p class="card-text">體型小巧，背部有美麗的三角形或類文字斑紋。每逢大雨過後的春夏夜晚，大池周邊的草叢與積水處便會傳出牠們密集的宏亮鳴叫聲。</p>
                        <button class="cta-btn" onclick="openModal('modal-animal-6')">詳細動物紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
            </div>
        </section>

        <section id="eco-bird" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：鳥類 (張佳琪)</h2>
                <p>探索棲息並常駐於龍潭大池的六大核心指標鳥類</p>
            </div>
            <div class="card-grid">
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Gallinula_chloropus_-Westerpark_-Amsterdam-8.jpg/250px-Gallinula_chloropus_-Westerpark_-Amsterdam-8.jpg" alt="紅冠水雞"></div>
                    <div class="card-body">
                        <h3>紅冠水雞</h3>
                        <p class="card-text">大池最常見的指標性留鳥，身體黑褐色，額頭具有鮮紅色的額甲，擅長在挺水植物間穿梭與築巢。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-1')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Litle_Grebe_-_Tachybaptus_ruficollis.jpg/250px-Litle_Grebe_-_Tachybaptus_ruficollis.jpg" alt="小鸊鷉"></div>
                    <div class="card-body">
                        <h3>小鸊鷉</h3>
                        <p class="card-text">體型小巧如鴨，是埤塘中的潛水高手。遇到驚嚇時會瞬間潛入水中，只在水面留下一陣漣漪。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-2')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/Great_Egret_%28Egretta_alba%29%2C_Skala_Kallonis%2C_Lesvos%2C_Greece%2C_19.04.2015_%2817449816992%29.jpg/250px-Great_Egret_%28Egretta_alba%29%2C_Skala_Kallonis%2C_Lesvos%2C_Greece%2C_19.04.2015_%2817449816992%29.jpg" alt="大白鷺"></div>
                    <div class="card-body">
                        <h3>大白鷺</h3>
                        <p class="card-text">優雅的涉禽，身形修長且全身披著潔白羽毛，常佇立於大池淺水灘中靜止不動，伺機捕食魚蝦。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-3')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ce/Black-crowned_Night_Heron_--_Nycticorax_nycticorax.jpg/330px-Black-crowned_Night_Heron_--_Nycticorax_nycticorax.jpg" alt="夜鷺"></div>
                    <div class="card-body">
                        <h3>夜鷺</h3>
                        <p class="card-text">俗稱「夜婆」，頭頂與背部羽毛呈藍黑色。屬於大池生態鏈的高級掠食者，多在黃昏及夜間活躍。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-4')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Alcedo_Atthis.jpg/250px-Alcedo_Atthis.jpg" alt="翠鳥"></div>
                    <div class="card-body">
                        <h3>翠鳥</h3>
                        <p class="card-text">擁有亮麗翠藍的羽色，俗稱魚狗。常靜停在池畔木棧道或樹枝上，隨後以高速衝入水中精準啣魚。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-5')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
                <div class="bird-card">
                    <div class="thumb-wrapper"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/23/Egretta_garzetta_EM1B0094_%2847336068462%29.jpg/330px-Egretta_garzetta_EM1B0094_%2847336068462%29.jpg" alt="小白鷺"></div>
                    <div class="card-body">
                        <h3>小白鷺</h3>
                        <p class="card-text">嘴黑、腳黑但腳趾為顯眼的黃綠色。覓食時會用腳攪動泥沙，驚嚇隱藏的小魚以利捕食。</p>
                        <button class="cta-btn" onclick="openModal('modal-bird-6')">詳細觀測紀錄 <i class="fa-solid fa-angle-right"></i></button>
                    </div>
                </div>
            </div>
        </section>

        <section id="eco-insect" class="page-section">
    <div class="section-title-box">
        <h2>生態調查：昆蟲 (張佳琪)</h2>
        <p>探索棲息於龍潭大池周邊濕地、草地與水域環境的六大指標性昆蟲</p>
    </div>

    <div class="card-grid">

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="臺灣大鍬形蟲">
            </div>
            <div class="card-body">
                <h3>臺灣大鍬形蟲</h3>
                <p class="card-text">臺灣特有種鍬形蟲，雄蟲具有大型彎曲大顎，是臺灣代表性的保育昆蟲。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-1')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="纖粉蝶">
            </div>
            <div class="card-body">
                <h3>纖粉蝶</h3>
                <p class="card-text">臺灣體型最小的粉蝶之一，飛行姿態輕盈優雅。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-2')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="大草螽">
            </div>
            <div class="card-body">
                <h3>大草螽</h3>
                <p class="card-text">全身翠綠色，具有極佳的保護色與跳躍能力。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-3')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="黃緣龍蝨">
            </div>
            <div class="card-body">
                <h3>黃緣龍蝨</h3>
                <p class="card-text">大型水生甲蟲，游泳能力強，是水域生態的重要指標。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-4')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="大刀螳">
            </div>
            <div class="card-body">
                <h3>大刀螳</h3>
                <p class="card-text">大型肉食性昆蟲，前足特化成鐮刀狀捕捉足。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-5')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

        <div class="bird-card">
            <div class="thumb-wrapper">
                <img src="data:image/jpeg;base64,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" alt="善變蜻蜓">
            </div>
            <div class="card-body">
                <h3>善變蜻蜓</h3>
                <p class="card-text">俗稱紅蜻蜓，是龍潭大池周邊常見的蜻蜓種類。</p>
                <button class="cta-btn" onclick="openModal('modal-insect-6')">詳細昆蟲紀錄 <i class="fa-solid fa-angle-right"></i></button>
            </div>
        </div>

    </div>
</section>

        <section id="eco-water" class="page-section">
            <div class="section-title-box">
                <h2>生態調查：水質觀測指標 (張佳琪)</h2>
                <p>監測龍潭大池水體健康的科學核心指標與國家地面水體分類</p>
            </div>
            <div class="water-grid">
                <div class="water-card">
                    <h3>物理與生物指標</h3>
                    <ul class="water-list">
                        <li><strong>水溫 (°C)：</strong> 影響水中溶氧量及水生生物的代謝速度。</li>
                        <li><strong>濁度 (NTU)：</strong> 水中懸浮微粒的多寡，反映水體混濁程度。</li>
                        <li><strong>大腸桿菌群 (CFU/100mL)：</strong> 常作為判定水質是否受到「糞便污染」的衛生指標。</li>
                    </ul>
                </div>
                <div class="water-card">
                    <h3>化學指標</h3>
                    <ul class="water-list">
                        <li><strong>酸鹼值 (pH)：</strong> 天然水體一般在 6.5 ~ 8.5 之間，過酸過鹼皆有害。</li>
                        <li><strong>溶氧量 (DO)：</strong> 水中溶解氧氣量，低於 2 mg/L 會導致水體發臭。</li>
                        <li><strong>生化需氧量 (BOD)：</strong> 微生物分解有機物需氧量，數值越高代表有機污染越重。</li>
                        <li><strong>化學需氧量 (COD)：</strong> 用化學氧化劑分解污染物的需氧量，反映工業廢水污染度。</li>
                    </ul>
                </div>
            </div>
            <div class="welcome-box" style="padding: 20px;">
                <h3 style="color:#38bdf8; margin-bottom:15px;"><i class="fa-solid fa-table"></i> 臺灣地面水體分類及適用用途</h3>
                <table class="struc-table">
                    <thead>
                        <tr>
                            <th style="width: 20%;">水體類別</th>
                            <th style="width: 80%;">適用用途 / 說明</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td style="color:#38bdf8; font-weight:bold;">甲類</td><td>一級公共用水（需經一般標準淨化即可飲用）、生態保育。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">乙類</td><td>二級公共用水（需經特定程序淨化）、一級水產用水（適合高經濟魚類養殖）。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">丙類</td><td>三級公共用水、二級水產用水、一級工業用水（製程用水）。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">丁類</td><td>灌溉用水、二級工業用水（冷卻用水）、環境保育。</td></tr>
                        <tr><td style="color:#38bdf8; font-weight:bold;">戊類</td><td>環境保育（僅能維持最基本的環境景觀，不發臭）。</td></tr>
                    </tbody>
                </table>
            </div>
        </section>

        <section id="hist-origin" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：劉亞蓁</span>
                <h2>歷史與文化：起源故事</h2><br>
                <p class="article-p">龍潭大池的歷史可追溯至清代開墾時期。桃園臺地地勢平坦卻缺乏大型河川，降雨後雨水容易迅速流失，因此早期客家先民為解決農業灌溉問題，利用天然低窪地築堤蓄水，逐漸形成今日龍潭大池的前身。</p>
                <p class="article-p">
早期大池周圍長滿菱角，因此被稱為「菱潭陂」。由於池面寬廣、水色清澈，加上地方流傳池中有靈異神蹟，因此後來又出現「靈潭陂」的稱呼。清代地方志與相關文獻中皆可見類似記載。
</p>

<p class="article-p">
相傳有居民在池中看見龍形倒影或雲霧盤旋於水面，因此逐漸將「靈潭」轉稱為「龍潭」。這類傳說雖帶有民間信仰色彩，但也反映當地居民對水資源的敬畏與依賴。隨著聚落發展，「龍潭」最終成為地方正式名稱，而龍潭大池也成為地方發展的重要核心。
</p>
            </div>
        </section>

        <section id="hist-life" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：劉亞蓁</span>
                <h2>歷史與文化：周邊生活</h2><br>
                <p class="article-p">
龍潭大池不僅是灌溉設施，更深刻影響居民的日常生活。早期居民依靠池水灌溉水稻、茶園與旱作農地，因此埤塘與農村經濟密不可分。每逢旱季，地方居民還需依照輪灌制度共同管理水資源。
</p>

<p class="article-p">
客家聚落大多沿著埤塘周邊形成。居民除了耕作之外，也利用池塘進行洗滌、捕魚及日常取水。龍潭大池因位於交通與聚落中心位置，逐漸成為地方居民交流與活動的重要公共空間。
</p>

<p class="article-p">
現代化之後，大池周圍逐漸發展為商業與觀光區。環湖步道、龍潭吊橋、南天宮及各式文化活動吸引大量遊客。雖然農業功能已減弱，但大池仍持續扮演地方文化記憶與社區認同的重要角色。
</p>
            </div>
        </section>

        <section id="hist-name" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：劉亞蓁</span>
                <h2>歷史與文化：地名考證</h2><br>
               <p class="article-p">
龍潭地名的演變反映了地方歷史與文化發展過程。根據地方文獻與學者研究，龍潭大池最早被稱為「菱潭陂」，原因是池中盛產菱角，形成特殊的水域景觀。
</p>

<p class="article-p">
後來因居民認為池水靈驗，或認為池中具有神秘氣息，因此逐漸出現「靈潭陂」的稱呼。在客家話中，「潭」指較大的水池或深水區，而「靈」則帶有神聖與神秘意涵。
</p>

<p class="article-p">
清末至日治時期，地方開始普遍使用「龍潭」一詞。民間傳說認為池中曾出現龍形倒影、龍氣升騰等景象，因此以「龍潭」取代原本的「靈潭」。今日龍潭區的行政名稱，即源自這座具有歷史意義的埤塘。
</p>
            </div>
        </section>

        <section id="water-sys" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：李芷嫻</span>
                <h2>地景與水利：地形與水源環境</h2><br>
                <h3>1. 地形環境</h3>
                <p class="article-p">龍潭大池位於桃園臺地，是典型的<strong>「埤塘地景」</strong>。桃園臺地因河川短、小、流速快，不容易直接取河水灌溉，因此先民利用天然低窪地與人工築堤形成埤塘蓄水。</p>
                <p class="article-p">龍潭大池原名包含：菱潭埤（因長滿菱角）、靈潭埤、龍潭埤，這也是後來成為「龍潭」地方行政區地名的重要來源。</p>
                
                <h3>2. 景觀構成</h3>
                <p class="article-p">整個龍潭大池的環境地景包含了：大面積水域、環湖堤岸、吊橋與忠義橋、南天宮人工島、周邊聚落與農田以及錯綜複雜的水圳系統。其中，<strong>「池塘 ＋ 聚落 ＋ 農田」</strong>的三位一體結構，正是桃園埤塘文化最核心的景觀特色。</p>
            </div>
        </section>

        <section id="water-struc" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：李芷嫻</span>
                <h2>地景與水利：關鍵水利構造</h2><br>
                <p class="article-p">龍潭大池並非純天然湖泊，而是透過「天然窪地 ＋ 人工引水 ＋ 築堤蓄水」所形成的永續灌溉系統。其上游水源主要引自老街溪上游、風櫃口埤，再經由渠道引入池中。以下為系統內六大關鍵水利建構細節：</p>
                
                <table class="struc-table">
                    <thead>
                        <tr>
                            <th style="width: 25%;">關鍵構造</th>
                            <th style="width: 75%;">核心功能與數據規模</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(1) 埤塘（蓄水）</td>
                            <td>負責儲存雨水與引水，於旱季時供應全面灌溉。龍潭大池的有效蓄水量高達約 165,000 立方公尺。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(2) 堤岸</td>
                            <td>主要用以防止池水外流並穩定大池水位，其大池堰堤全長約有 1200 公尺。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(3) 進水口</td>
                            <td>屬於「集水系統」的核心，負責將上游圳道的珍貴水源引導流入大池。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(4) 出水口（水門）</td>
                            <td>負責動態控制放水量並合理分配下游灌溉用水，大池下游設有兩個主要出水口。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(5) 水圳</td>
                            <td>水資源的輸送動脈，負責將池水精準輸送到各區農田。其中著名的龍潭圳全長達 3700 公尺。</td>
                        </tr>
                        <tr>
                            <td style="color:#38bdf8; font-weight:bold;">(6) 分汴水門</td>
                            <td>如同「水資源交通分流系統」，負責把主水圳的水量分流到不同的支線農地中，全區共設有約 40 處分汴水門。</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <section id="water-logic" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：李芷嫻</span>
                <h2>地景與水利：傳統水利分水邏輯</h2><br>
                <p class="article-p">龍潭大池歷久不衰的配水與分水智慧，可以被直觀地簡化為以下的高效線性流程：</p>
                
                <div class="flowchart">
                    <span class="flow-step">上游來水</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">進水口</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">龍潭大池蓄水</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">水門控制放流</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">主水圳</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">分汴水門</span> <span class="arrow"><i class="fa-solid fa-arrow-right"></i></span>
                    <span class="flow-step">各農田精準灌溉</span>
                </div>

                <h3 style="margin-top: 20px;">分水系統四大核心運作原理：</h3>
                <ul style="padding-left: 20px; margin-top: 10px;">
                    <li style="margin-bottom: 10px;"><strong>1. 先蓄後放：</strong>於豐水雨季提前存蓄水源，在枯水旱季時再穩定釋放供應。</li>
                    <li style="margin-bottom: 10px;"><strong>2. 高處到低處（重力流）：</strong>完美利用地理自然地勢的高低落差，使水流依重力自然向下流動，無需耗費大量電力與抽水機，兼顧低碳環保。</li>
                    <li style="margin-bottom: 10px;"><strong>3. 分區輪灌機制：</strong>各區農田不重疊搶水，採取依序、排程放水，徹底解決上游搶水、下游無水可用的分配不均問題。</li>
                    <li style="margin-bottom: 10px;"><strong>4. 水門流量精密控制：</strong>因應各農地大小，靠調整水門開關大小與水位高度決定分流量，體現古代與現代交織的水利環境工程智慧。</li>
                </ul>
            </div>
        </section>

        <section id="now-land" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：劉亞蓁</span>
                <h2>變遷與現狀：土地變化</h2><br>
                <p class="article-p">
龍潭大池周邊土地利用型態歷經明顯變遷。清代至日治時期，大池主要服務農業灌溉需求，周圍多為水田、茶園及客家聚落，形成典型的埤塘農業景觀。
</p>

<p class="article-p">
戰後人口增加與都市發展加速，部分農地逐漸轉變為住宅區、商業區及交通設施。原本廣大的農業空間逐漸縮減，大池的灌溉功能也相對降低。
</p>

<p class="article-p">
近年政府推動環境整治與景觀改善工程，興建環湖步道、觀景平台與生態設施，使龍潭大池由傳統農業埤塘轉型為兼具生態保育、觀光休閒及環境教育功能的重要公共空間。
</p>
            </div>
        </section>

        <section id="now-func" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：劉亞蓁</span>
                <h2>變遷與現狀：轉型功能</h2><br>
                <p class="article-p">現代龍潭大池結合了觀光、滯洪、多層複合濾料（MSL）及礫間水質淨化科技，成功轉型為多元綠色環境教育場所。</p>
            </div>
        </section>

        <section id="now-issue" class="page-section">
            <div class="welcome-box text-card">
                <span class="author">撰寫者：劉亞蓁</span>
                <h2>變遷與現狀：面臨挑戰</h2><br>
               <p class="article-p">
雖然龍潭大池已完成多項環境改善工程，但仍面臨水質優養化問題。當氮、磷等營養鹽過量進入水體時，容易造成藻類大量繁殖，進而降低溶氧量並影響水域生態健康。
</p>

<p class="article-p">
外來入侵種也是重要挑戰之一。例如布袋蓮與克氏原螯蝦等生物可能快速擴散，壓縮原生物種生存空間，破壞埤塘既有生態平衡。
</p>

<p class="article-p">
此外，都市開發所帶來的人為干擾、觀光壓力以及氣候變遷造成的極端降雨與乾旱現象，也對大池的水資源管理形成新的考驗。未來如何兼顧觀光發展、生態保育與文化保存，將是龍潭大池永續經營的重要課題。
</p>
            </div>
        </section>


        <section id="task-choice" class="page-section">
            <div class="section-title-box">
                <h2>互動任務：環境教育知識測驗</h2>
                <p>請點擊下方選項進行即時檢測，系統將自動給予評分與解析。</p>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q1. 龍潭大池的主要水源主要來自下列哪一個系統？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-origin', '大圳與老街溪系統')">A. 自來水加壓供水系統</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-origin', '大圳與老街溪系統')">B. 工業廢水循環系統</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-origin', '大圳與老街溪系統')">C. 大圳與老街溪引水系統</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-origin', '大圳與老街溪系統')">D. 深層地下溫泉系統</button>
                <div id="ans-origin" class="analysis-box"><strong>標準答案：C</strong><br>解析：根據地景水利調查文本，大池主要依靠渠道引入老街溪上游及風櫃口埤之水源，而非自來水、工業水或溫泉。</div>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q2. 龍潭大池最早在先民開發、闢建時，最主要的初衷與核心目的為何？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-1', 'B')">A. 水力發電與工業供水</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-1', 'B')">B. 蓄水灌溉農田周邊</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-1', 'B')">C. 開闢海運與商務貿易港口</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-1', 'B')">D. 專門用於大型高密度捕魚與水產養殖</button>
                <div id="ans-1" class="analysis-box"><strong>標準答案：B</strong><br>解析：先民因應桃園臺地蓄水不易，利用低窪地築堤，最主要的原始核心功能即為農業灌溉。</div>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q3. 負責在主水圳上將珍貴的水資源流向各個不同區塊農地的關鍵「交通分流構造」是什麼？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-2', 'D')">A. 生態曝氣池</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-2', 'D')">B. 環池阻水堰堤</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-2', 'D')">C. 大型發電抽水機</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-2', 'D')">D. 分汴水門</button>
                <div id="ans-2" class="analysis-box"><strong>標準答案：D</strong><br>解析：全區擁有大約 40 處的「分汴水門」，其功能正是如同水流的紅綠燈，將主圳的水流合理分撥至不同農地。</div>
            </div>

            <div class="quiz-item">
                <div class="quiz-title">Q4. 龍潭大池下游的水圳在輸送水資源時，主要是依循什麼科學原理來達成永續節能？</div>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-3', 'A')">A. 太陽能光電加壓幫浦</button>
                <button class="option-btn" onclick="checkRadio(this, true, 'ans-3', 'A')">B. 利用地形自然地勢的重力高低差</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-3', 'A')">C. 大規模機械潮汐逆流泵</button>
                <button class="option-btn" onclick="checkRadio(this, false, 'ans-3', 'A')">D. 風力虹吸虹吸效應</button>
                <div id="ans-3" class="analysis-box"><strong>標準答案：B</strong><br>解析：大池設計完美順應地形，讓水自然從高處流向低處，因此不需要抽水機就能自然灌溉。</div>
            </div>
        </section>

        <section id="task-fill" class="page-section">
            <div class="section-title-box">
                <h2>互動任務：填空檢測與簡答思維</h2>
                <p>請在填空欄中鍵入正確名詞並點擊檢查；簡答思考題可點擊下方按鈕對照參考答案。</p>
            </div>

            <div class="welcome-box">
                <h3 style="color:#38bdf8; margin-bottom:15px;"><i class="fa-solid fa-pen-to-square"></i> 水利構造與脈絡填空題</h3>
                <ol style="padding-left: 20px; line-height: 2;">
                    <li>龍潭大池位居桃園臺地，屬於台灣非常具有在地特色的典型「 <input type="text" id="bk-1" class="fill-blank-input" placeholder="兩個字"> 」地景。</li>
                    <li>大池在調節水資源時，主要是運用智慧採取「先 <input type="text" id="bk-2" class="fill-blank-input" style="width:50px;" placeholder="字"> 後 <input type="text" id="bk-3" class="fill-blank-input" style="width:50px;" placeholder="字"> 」的邏輯彈性供水。</li>
                    <li>在水利系統中，具備動態調控大池放水量、重新分配重要水流的渠道卡閘設施，我們稱之為「 <input type="text" id="bk-4" class="fill-blank-input" placeholder="兩個字"> 」。</li>
                    <li>長度達 3700 公尺的龍潭圳，在整體架構中肩負的核心功能，是將大池水精準送到下游的「 <input type="text" id="bk-5" class="fill-blank-input" placeholder="兩個字"> 」中。</li>
                </ol>
                <button class="check-btn" onclick="validateBlanks()">提交檢查填空答案</button>
                <span id="bk-result" class="result-text"></span>
            </div>

            <div class="welcome-box">
                <h3 style="color:#38bdf8; margin-bottom:15px;"><i class="fa-solid fa-comments"></i> 環境與人文深度簡答思考</h3>
                
                <div style="margin-bottom: 20px;">
                    <p style="font-weight:bold; color:#cbd5e1;">(1) 假設早期在沒有埤塘系統的輔助下，桃園臺地的農業發展將會遭遇什麼致命性的困境？</p>
                    <textarea id="box-1" class="text-area-answer" placeholder="請在此輸入您的見解與回答..."></textarea>
                    <button class="check-btn" onclick="toggleRef('reply-1')">顯示 / 隱藏 參考解答</button>
                    <div id="reply-1" class="reference-answer">
                        <strong>環境學科參考觀點：</strong> 農業將面臨毀滅性的常態缺水。由於桃園臺地地勢特殊，河川太短且流速過快，降雨很快就會流入大海、無法蓄留。少了埤塘在雨季進行「先蓄後放」，到旱季時農田將完全乾涸。
                    </div>
                </div>

                <div style="margin-bottom: 20px;">
                    <p style="font-weight:bold; color:#cbd5e1;">(2) 在全面高度都市化的現代社會中，你認為我們還需要完整保留傳統埤塘嗎？理由為何？</p>
                    <textarea id="box-2" class="text-area-answer" placeholder="請在此輸入您的見解與回答..."></textarea>
                    <button class="check-btn" onclick="toggleRef('reply-2')">顯示 / 隱藏 參考解答</button>
                    <div id="reply-2" class="reference-answer">
                        <strong>環境學科參考觀點：</strong> 依然極度需要。現代埤塘已由單純灌溉成功轉型為多元韌性功能，例如：在暴雨時充當「都市防洪滯洪池」調節洪峰、減緩熱島效應以調節都市微氣候，更是珍貴的都市綠帶、生態保育及環境教育的活教材。
                    </div>
                </div>

                <div>
                    <p style="font-weight:bold; color:#cbd5e1;">(3) 現代的龍潭大池除了延續部分傳統水利功能，還延伸被賦予了哪些當代的永續新使命？</p>
                    <textarea id="box-3" class="text-area-answer" placeholder="請在此輸入您的見解與回答..."></textarea>
                    <button class="check-btn" onclick="toggleRef('reply-3')">顯示 / 隱藏 參考解答</button>
                    <div id="reply-3" class="reference-answer">
                        <strong>環境學科參考觀點：</strong> 現代大池不僅是著名的觀光休閒與文化地標（南天宮、吊橋），更導入現代環保科技（如多層複合濾料MSL、生態礫間曝氣）進行主動式水質再淨化，成為實踐環境友善與水環境教育的核心基地。
                    </div>
                </div>

            </div>
        </section>

    </main>

    <div id="customModal" class="custom-modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <h2 id="modalTitle">標題</h2>
            <hr>
            <p id="modalBody">詳細紀錄內容載入中...</p>
        </div>
    </div>

    <footer class="main-footer">
        <p>&copy; 2026 龍潭大池埤塘數位資料庫. 專題環境教育平台. All Rights Reserved.</p>
    </footer>

    <script>
        // 1. 單頁式網頁分頁切換控制 (SPA 邏輯)
        function showPage(sectionId) {
            // 隱藏所有分頁
            const sections = document.querySelectorAll('.page-section');
            sections.forEach(sec => sec.classList.remove('active'));
            
            // 秀出對應的分頁
            const targetSection = document.getElementById(sectionId);
            if (targetSection) {
                targetSection.classList.add('active');
                // 換頁時自動置頂
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }
        }

        // 2. 原生態卡片 Modal 互動控制
        function openModal(type) {
            const modal = document.getElementById('customModal');
            const title = document.getElementById('modalTitle');
            const body = document.getElementById('modalBody');
            
            // 植物、動物與鳥類詳情文本映射庫
            const infoData = {
                'modal-plant-1': { t: '台灣萍蓬草 (指標物種)', b: '學名：Nuphar shimadae。台灣特有種浮葉性水生植物。因面臨棲地喪失，野生群落極度稀少。大池生態池將其作為重點復育指標，全年可見金黃色球狀小花。' },
                'modal-plant-2': { t: '香蒲 (水蠟燭)', b: '常見的挺水植物，擁有極佳的氮磷吸附與重金屬過濾能力，能有效穩定水質品質，減緩水體優養化速度。' },
                'modal-plant-3': { t: '風車草 (輪傘草)', b: '原產於非洲，現已本土化。生命力極為頑強。其放射狀的苞片外型神似風車，是絕佳的固堤與景觀綠化植栽。' },
                'modal-plant-4': { t: '蓮 (荷花)', b: '挺水經濟兼具觀賞價值之植物，大池景觀區適度栽種，其發達的地下莖能固化底泥，葉片則能阻擋部分烈日輻射減少藻類過度曝曬。' },
                'modal-plant-5': { t: '鴨舌草', b: '雨久花科原生植物，葉形似鴨舌。傳統水田與埤塘濕地極為常見，對肥沃水體適應力高，藍紫色小花極富鄉土生態特色。' },
                'modal-plant-6': { t: '布袋蓮 (強勢外來種警告)', b: '原產於南美洲。雖然具備極高的高效有機物吸附力，但繁衍速度過快會徹底阻絕水面空氣與陽光，導致水下魚類窒息，需定期執行機械撈除維護。' },
                'modal-animal-1': { t: '原生指標：斑龜', b: '台灣原生種澤龜，因頭部有大量翠綠色平行線條而得名。喜好日光浴，是埤塘水域生態鏈中不可或缺的原生清道夫。' },
                'modal-animal-2': { t: '蓬萊草蜥', b: '台灣特有種。活動於岸邊草叢，由於對過度水泥化的地貌非常敏感，大池保留的周邊自然土質草帶提供了其絕佳繁衍棲地。' },
                'modal-animal-3': { t: '貢德氏赤蛙', b: '因其宛如犬吠的巨大「狂、狂」聲而聞名。體型大，偏好具有茂密挺水植物的隱蔽水域，是水體環境無受嚴重毒性污染的生態指標。' },
                'modal-animal-4': { t: '外來種警告：克氏原螯蝦 (美國螯蝦)', b: '極具破壞性的外來入侵種。因棄養進入大池，其強大的掘穴習性會破壞土堤、剪碎挺水植物根系，嚴重威脅本土原生生態平衡。' },
                'modal-animal-5': { t: '黑眶蟾蜍', b: '具發達耳後腺與毒腺，對環境耐受度極高。夜間會出沒在路燈及大池周圍步道捕捉飛蛾、害蟲，具備重要的都市生物防治功能。' },
                'modal-animal-6': { t: '小雨蛙', b: '體型迷你的姬蛙科成員。雖然身形僅約 2-3 公分，但鳴囊聲音宏亮，大雨過後在大池草岸區的交配鳴叫聲是經典的埤塘夜間交響樂。' },
                'modal-bird-1': { t: '紅冠水雞', b: '大池常駐留鳥。腳趾無蹼但擅游泳，會在香蒲或風車草叢深處編織浮巢，對大池周邊的人為活動具備一定程度的適應力。' },
                'modal-bird-2': { t: '小鸊鷉 (水鳥潛水王)', b: '體型圓胖的小型水鳥。主食小魚蝦，擁有極高的潛水本領，常在水面消失後於數十公尺外重新浮出。' },
                'modal-bird-3': { t: '大白鷺 (涉禽)', b: '冬季候鳥或部分留鳥。擁有修長黑色的涉水腿，大池大面積的淺灘與清澈度是牠們駐足捕食的關鍵環境。' },
                'modal-bird-4': { t: '夜鷺 (暗夜掠食者)', b: '夜行性鷺科。白天多在橋墩或樹蔭下蹲縮，黃昏至夜間則展現極高的捕魚效率，是大池高階的鳥類掠食者。' },
                'modal-bird-5': { t: '翠鳥 (翡翠衝刺者)', b: '俗稱魚狗。背部羽毛閃爍金屬翠藍。牠們需要乾淨且魚類豐富的池塘，大池近年水質淨化有成，使其出現頻率大幅上升。' },
                'modal-bird-6': { t: '小白鷺', b: '全年可見。與大白鷺不同的是其黃色的腳趾與黑色的嘴喙。會利用「腳趾抖動」的集體智慧戰術將泥沙底的魚驚嚇趕出以捕食。' },
                'modal-insect-1': {
    t: '臺灣大鍬形蟲',
    b: '臺灣特有種鍬形蟲，雄蟲具有大型大顎，是臺灣著名保育昆蟲。'
},

'modal-insect-2': {
    t: '纖粉蝶',
    b: '臺灣最小型粉蝶之一，常見於草地與灌木叢環境。'
},

'modal-insect-3': {
    t: '大草螽',
    b: '具有極佳保護色的大型螽斯，常活動於草叢中。'
},

'modal-insect-4': {
    t: '黃緣龍蝨',
    b: '大型水生甲蟲，後足具有游泳毛，是優良水域環境指標。'
},

'modal-insect-5': {
    t: '大刀螳',
    b: '大型肉食昆蟲，利用鐮刀狀前足捕捉獵物。'
},

'modal-insect-6': {
    t: '善變蜻蜓',
    b: '俗稱紅蜻蜓，幼蟲生活於水中，常被視為健康水域的象徵。'
},
            };

            if(infoData[type]) {
                title.textContent = infoData[type].t;
                body.textContent = infoData[type].b;
                modal.style.display = 'flex';
            }
        }

        function closeModal() {
            document.getElementById('customModal').style.display = 'none';
        }

        // 3. 環境選擇題即時反饋邏輯
        function checkRadio(button, isCorrect, divId, targetText) {
            const box = button.parentElement;
            const allBtns = box.querySelectorAll('.option-btn');
            
            // 清除本題目前的紅綠狀態
            allBtns.forEach(b => b.classList.remove('correct', 'wrong'));

            if (isCorrect) {
                button.classList.add('correct');
            } else {
                button.classList.add('wrong');
                // 自動幫使用者把正確答案標示為綠色
                allBtns.forEach(b => {
                    if(b.textContent.includes(targetText) || b.textContent.startsWith(targetText)) {
                        b.classList.add('correct');
                    }
                });
            }
            // 顯示該題詳盡解析卡片
            document.getElementById(divId).style.display = 'block';
        }

        // 4. 水利填空題即時對錯驗證
        function validateBlanks() {
            const v1 = document.getElementById('bk-1').value.trim();
            const v2 = document.getElementById('bk-2').value.trim();
            const v3 = document.getElementById('bk-3').value.trim();
            const v4 = document.getElementById('bk-4').value.trim();
            const v5 = document.getElementById('bk-5').value.trim();

            let scoreCount = 0;
            if(v1 === '埤塘') scoreCount++;
            if(v2 === '蓄') scoreCount++;
            if(v3 === '放') scoreCount++;
            if(v4 === '水門') scoreCount++;
            if(v5 === '農田') scoreCount++;

            const resultLabel = document.getElementById('bk-result');
            if(scoreCount === 5) {
                resultLabel.style.color = '#10b981';
                resultLabel.textContent = '🎉 全對！完美掌握李芷嫻同學整理的水利工程智慧！';
            } else {
                resultLabel.style.color = '#f97316';
                resultLabel.textContent = `答對了 ${scoreCount} 題。正確完整解答序列為：1.埤塘、2.蓄、3.放、4.水門、5.農田。再試一次吧！`;
            }
        }

        // 5. 申論思考題答案切換
        function toggleRef(id) {
            const block = document.getElementById(id);
            block.style.display = (block.style.display === 'none' || block.style.display === '') ? 'block' : 'none';
        }
    </script>
</body>
</html
