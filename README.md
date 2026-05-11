<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Steam游戏计划管理器 | 增强版</title>
    <style>
        * {
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', system-ui, 'Inter', -apple-system, BlinkMacSystemFont, 'Roboto', sans-serif;
            background: #1a2a32;
            color: #eef4ff;
            margin: 0;
            padding: 24px 16px;
            min-height: 100vh;
        }
        .container {
            max-width: 1300px;
            margin: 0 auto;
        }
        h1 {
            font-size: 1.9rem;
            font-weight: 600;
            background: linear-gradient(135deg, #e6f7ff, #81d4fa);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-align: center;
            margin-bottom: 0.5rem;
            letter-spacing: -0.3px;
        }
        h2, h3 {
            font-weight: 500;
            border-left: 5px solid #48cae4;
            padding-left: 16px;
            margin: 28px 0 16px 0;
            color: #f8fafc;
        }
        .search-section {
            background: #2d3e4b;
            backdrop-filter: blur(2px);
            border-radius: 28px;
            padding: 20px 24px;
            margin-bottom: 24px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
            transition: all 0.2s;
        }
        .search-wrapper {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            align-items: center;
        }
        .search-input-group {
            flex: 4;
            min-width: 180px;
            position: relative;
        }
        .search-input-group input {
            width: 100%;
            padding: 14px 18px;
            font-size: 1rem;
            border: none;
            border-radius: 60px;
            background: #1e2f3a;
            color: white;
            outline: none;
            transition: 0.2s;
            box-shadow: inset 0 1px 2px rgba(0,0,0,0.2);
        }
        .search-input-group input:focus {
            background: #2a3f4c;
            box-shadow: 0 0 0 2px #48cae4;
        }
        .btn-primary {
            background: #0f6b8c;
            padding: 12px 26px;
            border-radius: 60px;
            font-weight: 600;
            transition: 0.2s;
            border: none;
            color: white;
            cursor: pointer;
            font-size: 0.95rem;
        }
        .btn-primary:hover {
            background: #0a5a75;
            transform: scale(0.97);
        }
        .search-stats {
            margin-top: 12px;
            font-size: 0.85rem;
            color: #b0d9f1;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        .clear-search {
            background: none;
            border: none;
            color: #ffb347;
            cursor: pointer;
            font-size: 0.8rem;
            text-decoration: underline;
            padding: 4px 8px;
        }
        .grid-view {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        .game-card {
            background: #2c3f48;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 6px 14px rgba(0,0,0,0.3);
            transition: all 0.25s ease;
            display: flex;
            flex-direction: column;
            position: relative;
            backdrop-filter: blur(2px);
        }
        .game-card:hover {
            transform: translateY(-6px);
            box-shadow: 0 18px 28px rgba(0,0,0,0.4);
        }
        .game-image {
            width: 100%;
            height: 150px;
            object-fit: cover;
            background: #172a33;
        }
        .game-info {
            padding: 14px 12px;
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .game-name {
            font-weight: 700;
            font-size: 1.05rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin: 4px 0;
        }
        .tag {
            font-size: 11px;
            padding: 4px 10px;
            border-radius: 30px;
            font-weight: 500;
            background: #1e2f3a;
        }
        .tag-sale {
            background: #0f6b8c;
            color: #d4f1ff;
        }
        .tag-lowest {
            background: #2c7840;
            color: #ccffdd;
        }
        .controls {
            display: flex;
            justify-content: space-between;
            gap: 8px;
            margin-top: 8px;
        }
        button {
            background: #3a5e6e;
            border: none;
            padding: 6px 12px;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: 600;
            cursor: pointer;
            color: white;
            transition: 0.1s linear;
        }
        button:active {
            transform: scale(0.96);
        }
        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }
        .rank-indicator {
            position: absolute;
            top: 12px;
            left: 12px;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(4px);
            width: 32px;
            height: 32px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.1rem;
            border: 1px solid rgba(255,215,0,0.5);
        }
        .rank-1 .rank-indicator { background: #f5b042; color: #1f2a2e; box-shadow: 0 0 0 2px gold; }
        .rank-2 .rank-indicator { background: #b0c4de; color: #1e2f3a; }
        .rank-3 .rank-indicator { background: #cd7f32; color: white; }
        .top10-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(270px, 1fr));
            gap: 20px;
        }
        .monument-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            background: #1e353f;
            border-radius: 24px;
            padding: 16px;
            max-height: 480px;
            overflow-y: auto;
        }
        .completed-game {
            background: #2b4a58;
            border-radius: 20px;
            display: flex;
            align-items: center;
            gap: 16px;
            padding: 12px 16px;
            transition: 0.1s;
        }
        .completed-screenshot {
            width: 68px;
            height: 68px;
            object-fit: cover;
            border-radius: 16px;
            background: #0f2c36;
            border: 1px solid #48b5c9;
        }
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.75);
            backdrop-filter: blur(8px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .modal-card {
            background: #2c3f4a;
            border-radius: 32px;
            max-width: 440px;
            width: 90%;
            padding: 24px;
            box-shadow: 0 30px 40px rgba(0,0,0,0.5);
        }
        .hidden {
            display: none;
        }
        .empty-msg {
            text-align: center;
            color: #9bb7c4;
            padding: 32px;
            font-style: italic;
        }
        .btn-icon {
            background: #2c5a6e;
        }
        hr {
            border-color: #2d5b68;
        }
        footer {
            text-align: center;
            margin-top: 40px;
            font-size: 0.7rem;
            opacity: 0.6;
        }
        @media (max-width: 640px) {
            .grid-view, .top10-container { grid-template-columns: 1fr; }
            .search-wrapper { flex-direction: column; align-items: stretch; }
            .btn-primary { text-align: center; }
        }
    </style>
</head>
<body>
<div class="container">
    <h1>🎮 Steam 游戏计划管理器</h1>
    <div class="search-section">
        <div class="search-wrapper">
            <div class="search-input-group">
                <input type="text" id="search-input" placeholder="搜索游戏... (支持名称、标签筛选)" autocomplete="off">
            </div>
            <button id="search-btn" class="btn-primary">🔍 智能搜索</button>
            <button id="reset-search-btn" class="btn-icon" style="background:#405d6b;">🔄 重置</button>
        </div>
        <div class="search-stats" id="search-stats-panel">
            <span>✨ 试试输入 “史低” 或 “打折” 查看特惠游戏</span>
            <button id="clear-search-results" class="clear-search hidden">✖ 清除搜索结果</button>
        </div>
    </div>

    <h2>⭐ 收藏的游戏 <span id="fav-count" style="font-size:0.9rem;">(0/100)</span></h2>
    <div id="favorites-container" class="grid-view"></div>

    <h2>📋 待玩游戏 Top 10 <span style="font-size:0.8rem;">(拖拽排序体验↑↓)</span></h2>
    <div id="top10-container" class="top10-container"></div>

    <h2>🏆 通关纪念碑</h2>
    <div id="monument-container" class="monument-container"></div>
</div>

<div id="modal" class="modal-overlay hidden">
    <div class="modal-card">
        <h3 id="modal-title" style="margin-top:0;">上传通关截图</h3>
        <input type="file" id="screenshot-upload" accept="image/*" style="margin:15px 0; width:100%;">
        <div style="display:flex; gap:12px; justify-content:flex-end;">
            <button id="upload-btn" style="background:#2e8b57;">📸 上传</button>
            <button id="close-modal-btn">取消</button>
        </div>
    </div>
</div>

<script>
    // ---------- 增强游戏库 (更丰富的数据供搜索) ----------
    const baseGameLibrary = [
        { appid: 1, name: "CS2", header_image: "https://picsum.photos/id/1/250/140", price: 0, original_price: 0, lowest_price: 0, tags: ["射击", "竞技"] },
        { appid: 2, name: "DOTA2", header_image: "https://picsum.photos/id/2/250/140", price: 0, original_price: 0, lowest_price: 0, tags: ["MOBA", "策略"] },
        { appid: 3, name: "黑神话：悟空", header_image: "https://picsum.photos/id/104/250/140", price: 268, original_price: 268, lowest_price: 188, tags: ["动作", "神话", "RPG"] },
        { appid: 4, name: "塞尔达传说 王国之泪", header_image: "https://picsum.photos/id/106/250/140", price: 399, original_price: 399, lowest_price: 299, tags: ["开放世界", "冒险"] },
        { appid: 5, name: "艾尔登法环", header_image: "https://picsum.photos/id/169/250/140", price: 299, original_price: 299, lowest_price: 199, tags: ["魂系", "开放世界", "高难度"] },
        { appid: 6, name: "赛博朋克2077", header_image: "https://picsum.photos/id/96/250/140", price: 199, original_price: 299, lowest_price: 99, tags: ["RPG", "赛博朋克", "开放世界"] },
        { appid: 7, name: "GTA V", header_image: "https://picsum.photos/id/15/250/140", price: 58, original_price: 299, lowest_price: 29, tags: ["动作", "犯罪", "开放世界"] },
        { appid: 8, name: "文明6", header_image: "https://picsum.photos/id/22/250/140", price: 109, original_price: 268, lowest_price: 59, tags: ["策略", "回合制"] },
        { appid: 9, name: "战地风云5", header_image: "https://picsum.photos/id/77/250/140", price: 129, original_price: 399, lowest_price: 69, tags: ["射击", "战争"] },
        { appid: 10, name: "FIFA 23", header_image: "https://picsum.photos/id/31/250/140", price: 299, original_price: 399, lowest_price: 149, tags: ["体育", "足球"] },
        { appid: 11, name: "空洞骑士", header_image: "https://picsum.photos/id/42/250/140", price: 48, original_price: 68, lowest_price: 24, tags: ["类银河城", "动作", "独立"] },
        { appid: 12, name: "星露谷物语", header_image: "https://picsum.photos/id/120/250/140", price: 48, original_price: 48, lowest_price: 28, tags: ["模拟", "农场", "休闲"] },
        { appid: 13, name: "巫师3 狂猎", header_image: "https://picsum.photos/id/124/250/140", price: 63, original_price: 159, lowest_price: 31, tags: ["RPG", "开放世界", "剧情"] },
        { appid: 14, name: "只狼：影逝二度", header_image: "https://picsum.photos/id/225/250/140", price: 199, original_price: 268, lowest_price: 134, tags: ["动作", "忍者", "高难度"] },
        { appid: 15, name: "双人成行", header_image: "https://picsum.photos/id/26/250/140", price: 99, original_price: 198, lowest_price: 59, tags: ["合作", "冒险"] }
    ];
    
    // 增强版打折/史低标记函数 (实时判断)
    function enrichGameWithTags(game) {
        const isOnSale = game.price < game.original_price;
        const isLowest = game.price <= game.lowest_price && game.lowest_price > 0;
        return { ...game, isSale: isOnSale, isHistoricalLow: isLowest };
    }

    // 本地存储 keys
    let favorites = JSON.parse(localStorage.getItem('steam_favorites')) || [];
    let top10 = JSON.parse(localStorage.getItem('steam_top10')) || [];
    let completed = JSON.parse(localStorage.getItem('steam_completed')) || [];

    // 当前搜索模式: 是否显示搜索结果页 (覆盖收藏区)
    let activeSearchResults = null;  // 搜索结果数组或null
    
    // DOM 元素
    const searchInput = document.getElementById('search-input');
    const searchBtn = document.getElementById('search-btn');
    const resetSearchBtn = document.getElementById('reset-search-btn');
    const clearSearchResultsBtn = document.getElementById('clear-search-results');
    const searchStatsSpan = document.getElementById('search-stats-panel');
    const favoritesContainer = document.getElementById('favorites-container');
    const top10Container = document.getElementById('top10-container');
    const monumentContainer = document.getElementById('monument-container');
    const favCountSpan = document.getElementById('fav-count');
    const modal = document.getElementById('modal');
    const modalTitle = document.getElementById('modal-title');
    const screenshotUpload = document.getElementById('screenshot-upload');
    const uploadBtn = document.getElementById('upload-btn');
    const closeModalBtn = document.getElementById('close-modal-btn');
    
    let currentModalGameId = null;

    // ========== 核心渲染函数 (统一管理视图) ==========
    function renderFavoritesArea() {
        let gamesToDisplay = [];
        if (activeSearchResults !== null) {
            gamesToDisplay = activeSearchResults;
        } else {
            gamesToDisplay = favorites;
        }
        
        if (!gamesToDisplay.length) {
            const emptyText = activeSearchResults !== null ? "😵 没有找到匹配的游戏，试试其他关键词~" : "💡 暂无收藏，快使用上方搜索添加游戏吧！";
            favoritesContainer.innerHTML = `<div class="empty-msg">${emptyText}</div>`;
            return;
        }
        
        let html = '';
        gamesToDisplay.forEach(game => {
            const enriched = enrichGameWithTags(game);
            const isFavAlready = favorites.some(f => f.appid === game.appid);
            const isCurrentlyInSearchMode = activeSearchResults !== null;
            html += `
            <div class="game-card">
                <img src="${game.header_image || 'https://picsum.photos/id/0/250/140'}" alt="${game.name}" class="game-image" loading="lazy">
                <div class="game-info">
                    <div class="game-name">${escapeHtml(game.name)}</div>
                    <div class="tags">
                        ${enriched.isSale ? '<span class="tag tag-sale">🔥 打折中</span>' : ''}
                        ${enriched.isHistoricalLow ? '<span class="tag tag-lowest">📉 历史低价</span>' : ''}
                        ${game.tags ? game.tags.slice(0,2).map(t => `<span class="tag" style="background:#2c6e6e;">#${t}</span>`).join('') : ''}
                    </div>
                    <div class="controls">
                        ${!isCurrentlyInSearchMode ? `<button onclick="removeFromFavorites(${game.appid})">🗑️ 移除</button>
                        <button onclick="addToTop10FromFav(${game.appid})" ${top10.length>=10 ? 'disabled' : ''}>📌 加入Top10</button>` : 
                        `<button onclick="addToFavoritesFromSearch(${game.appid})" ${isFavAlready ? 'disabled' : ''}>${isFavAlready ? '✓ 已收藏' : '⭐ 收藏'}</button>
                        <button onclick="openGameInSteam(${game.appid})">🔗 详情</button>`}
                    </div>
                </div>
            </div>`;
        });
        favoritesContainer.innerHTML = html;
        updateFavCount();
    }
    
    function updateFavCount() {
        favCountSpan.innerText = `(${favorites.length}/100)`;
    }
    
    // Top10渲染
    function renderTop10() {
        if (!top10.length) {
            top10Container.innerHTML = '<div class="empty-msg">🎯 暂无游戏，请从收藏中添加至Top10</div>';
            return;
        }
        let html = '';
        top10.forEach((game, idx) => {
            const rankClass = idx < 3 ? `rank-${idx+1}` : '';
            const enriched = enrichGameWithTags(game);
            html += `
            <div class="game-card ${rankClass}" data-index="${idx}">
                <div class="rank-indicator">${idx+1}</div>
                <img src="${game.header_image}" class="game-image" loading="lazy">
                <div class="game-info">
                    <div class="game-name">${escapeHtml(game.name)}</div>
                    <div class="tags">
                        ${enriched.isSale ? '<span class="tag tag-sale">打折中</span>' : ''}
                        ${enriched.isHistoricalLow ? '<span class="tag tag-lowest">史低</span>' : ''}
                    </div>
                    <div class="controls">
                        <button onclick="moveUpTop10(${idx})">⬆️ 上移</button>
                        <button onclick="moveDownTop10(${idx})">⬇️ 下移</button>
                        <button onclick="finishGame(${game.appid})" style="background:#4c9f70;">✅ 通关</button>
                    </div>
                </div>
            </div>`;
        });
        top10Container.innerHTML = html;
    }
    
    // 纪念碑渲染
    function renderMonument() {
        if (!completed.length) {
            monumentContainer.innerHTML = '<div class="empty-msg">🏅 暂无通关游戏，通关的荣耀会记录在此～</div>';
            return;
        }
        let html = '';
        completed.forEach(game => {
            html += `
            <div class="completed-game">
                <img src="${game.screenshot || 'https://picsum.photos/id/1/68/68'}" class="completed-screenshot" alt="截图">
                <div style="flex:1">
                    <strong>${escapeHtml(game.name)}</strong>
                    <div style="font-size: 11px; color: #b9e6ff;">🎉 通关日: ${game.finishDate || '未知'}</div>
                </div>
                <button onclick="openScreenshotModal(${game.appid})">📷 截图</button>
            </div>`;
        });
        monumentContainer.innerHTML = html;
    }
    
    function fullRefresh() {
        renderFavoritesArea();
        renderTop10();
        renderMonument();
        saveData();
    }
    
    // ========== 搜索增强逻辑 ==========
    function performSmartSearch() {
        let rawQuery = searchInput.value.trim();
        if (rawQuery === "") {
            resetSearchMode();
            return;
        }
        const lowerQuery = rawQuery.toLowerCase();
        // 特殊语义: "史低" "打折" 等智能过滤
        let filteredGames = [...baseGameLibrary];
        // 先根据输入文本过滤名称+标签
        if (!(lowerQuery.includes("史低") || lowerQuery.includes("历史低价") || lowerQuery.includes("打折"))) {
            filteredGames = baseGameLibrary.filter(game => 
                game.name.toLowerCase().includes(lowerQuery) || 
                (game.tags && game.tags.some(tag => tag.toLowerCase().includes(lowerQuery)))
            );
        } else {
            // 智能特惠模式: 找出所有当前打折 或者 达到史低的游戏
            filteredGames = baseGameLibrary.filter(game => {
                const enriched = enrichGameWithTags(game);
                if (lowerQuery.includes("史低") || lowerQuery.includes("历史低价")) {
                    return enriched.isHistoricalLow;
                }
                if (lowerQuery.includes("打折")) {
                    return enriched.isSale;
                }
                return false;
            });
        }
        
        // 附加去重但保留完整对象
        const uniqueResults = filteredGames.filter((v,i,a)=>a.findIndex(t=>t.appid===v.appid)===i);
        if (uniqueResults.length === 0) {
            activeSearchResults = [];
            renderFavoritesArea();
            searchStatsSpan.innerHTML = `<span>😭 没有找到“${escapeHtml(rawQuery)}”相关游戏，试试别的关键词~</span> <button class="clear-search" id="inline-clear">清除搜索</button>`;
            const inlineBtn = document.getElementById('inline-clear');
            if(inlineBtn) inlineBtn.onclick = resetSearchMode;
            clearSearchResultsBtn.classList.remove('hidden');
            return;
        }
        
        activeSearchResults = uniqueResults;
        renderFavoritesArea();
        searchStatsSpan.innerHTML = `<span>🔍 找到 ${uniqueResults.length} 款游戏，当前为搜索结果模式</span>`;
        clearSearchResultsBtn.classList.remove('hidden');
        // 绑定清除搜索按钮事件确保可用
        if(clearSearchResultsBtn) clearSearchResultsBtn.onclick = resetSearchMode;
    }
    
    function resetSearchMode() {
        activeSearchResults = null;
        searchInput.value = '';
        renderFavoritesArea();
        searchStatsSpan.innerHTML = `<span>✨ 试试输入 “史低” 或 “打折” 查看特惠游戏</span>`;
        clearSearchResultsBtn.classList.add('hidden');
    }
    
    // 从搜索结果添加到收藏
    window.addToFavoritesFromSearch = function(appid) {
        if (favorites.length >= 100) {
            alert('收藏已达上限100个！');
            return;
        }
        const gameSource = activeSearchResults || baseGameLibrary;
        const game = gameSource.find(g => g.appid === appid);
        if (game && !favorites.some(f => f.appid === appid)) {
            favorites.push({ ...game });
            saveData();
            // 若当前在搜索模式，刷新搜索结果区域 (收藏按钮状态变更)
            renderFavoritesArea();
            updateFavCount();
        }
    };
    
    // 从收藏区添加至Top10 (原有逻辑)
    window.addToTop10FromFav = function(appid) {
        if (top10.length >= 10) {
            alert('Top10已满 (最多10款)');
            return;
        }
        const game = favorites.find(f => f.appid === appid);
        if (game && !top10.some(t => t.appid === appid)) {
            top10.push({ ...game });
            saveData();
            renderTop10();
        } else if (top10.some(t => t.appid === appid)) {
            alert('此游戏已在Top10列表中');
        }
    };
    
    window.removeFromFavorites = function(appid) {
        favorites = favorites.filter(f => f.appid !== appid);
        saveData();
        if (activeSearchResults !== null) {
            // 搜索结果视图不变，但重新渲染更新收藏按钮显示
            renderFavoritesArea();
        } else {
            renderFavoritesArea();
        }
        updateFavCount();
    };
    
    window.openGameInSteam = function(appid) {
        window.open(`https://store.steampowered.com/app/${appid}`, '_blank');
    };
    
    // Top10 移动
    window.moveUpTop10 = function(index) {
        if (index > 0) {
            [top10[index], top10[index-1]] = [top10[index-1], top10[index]];
            saveData();
            renderTop10();
        }
    };
    window.moveDownTop10 = function(index) {
        if (index < top10.length-1) {
            [top10[index], top10[index+1]] = [top10[index+1], top10[index]];
            saveData();
            renderTop10();
        }
    };
    
    // 完成游戏 (通关)
    window.finishGame = function(appid) {
        const idx = top10.findIndex(g => g.appid === appid);
        if (idx !== -1) {
            const finished = top10.splice(idx,1)[0];
            finished.finishDate = new Date().toLocaleDateString('zh-CN');
            finished.screenshot = finished.screenshot || null;
            completed.unshift(finished);
            saveData();
            renderTop10();
            renderMonument();
        } else {
            alert('游戏不在待玩列表中');
        }
    };
    
    // 截图模态框相关
    window.openScreenshotModal = function(appid) {
        const gameInCompleted = completed.find(g => g.appid === appid);
        if (gameInCompleted) {
            currentModalGameId = appid;
            modalTitle.innerText = `上传「${escapeHtml(gameInCompleted.name)}」通关截图`;
            modal.classList.remove('hidden');
        } else {
            alert('未找到对应的通关记录');
        }
    };
    
    uploadBtn.onclick = () => {
        if (!screenshotUpload.files.length) {
            alert('请选择一张图片');
            return;
        }
        const file = screenshotUpload.files[0];
        if (!file.type.startsWith('image/')) {
            alert('只支持图片格式');
            return;
        }
        const reader = new FileReader();
        reader.onload = function(ev) {
            const imgData = ev.target.result;
            const targetGame = completed.find(g => g.appid === currentModalGameId);
            if (targetGame) {
                targetGame.screenshot = imgData;
                saveData();
                renderMonument();
            }
            closeModal();
        };
        reader.readAsDataURL(file);
    };
    
    function closeModal() {
        modal.classList.add('hidden');
        screenshotUpload.value = '';
        currentModalGameId = null;
    }
    closeModalBtn.onclick = closeModal;
    
    // 保存数据
    function saveData() {
        localStorage.setItem('steam_favorites', JSON.stringify(favorites));
        localStorage.setItem('steam_top10', JSON.stringify(top10));
        localStorage.setItem('steam_completed', JSON.stringify(completed));
    }
    
    function escapeHtml(str) {
        if(!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if(m === '&') return '&amp;';
            if(m === '<') return '&lt;';
            if(m === '>') return '&gt;';
            return m;
        });
    }
    
    // 全局新增重置搜索监听
    resetSearchBtn.onclick = () => resetSearchMode();
    searchBtn.onclick = () => performSmartSearch();
    searchInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') performSmartSearch();
    });
    
    // 额外初始化数据兼容（如果没有top10，预设演示）
    if (top10.length === 0 && favorites.length === 0) {
        // 可选默认演示一些数据便于展示，但不强制
        const demoFavs = baseGameLibrary.slice(0,3);
        favorites = demoFavs.map(g => ({ ...g }));
        saveData();
    }
    
    init();
    function init() {
        updateFavCount();
        fullRefresh();
    }
</script>
</body>
</html>
