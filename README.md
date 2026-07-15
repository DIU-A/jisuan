<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>涂布工艺参数计算器</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft Yahei", sans-serif;
        }
        body {
            background-color: #f5f7fa;
            padding: 30px 20px;
            color: #333;
        }
        .container {
            max-width: 700px;
            margin: 0 auto;
        }
        h1 {
            color: #2563eb;
            text-align: center;
            margin-bottom: 10px;
            font-size: 24px;
            font-weight: 600;
        }
        .current-time {
            text-align: center;
            color: #666;
            font-size: 14px;
            margin-bottom: 20px;
        }
        /* 切换菜单 */
        .tab-nav {
            display: flex;
            background: #fff;
            border-radius: 12px 12px 0 0;
            box-shadow: 0 2px 15px rgba(0,0,0,0.08);
            overflow: hidden;
        }
        .tab-item {
            flex: 1;
            text-align: center;
            padding: 14px 0;
            font-size: 15px;
            cursor: pointer;
            color: #666;
            transition: all 0.2s;
            border-bottom: 2px solid transparent;
        }
        .tab-item.active {
            color: #2563eb;
            font-weight: 600;
            border-bottom-color: #2563eb;
            background-color: #eff6ff;
        }
        .tab-item:hover {
            background-color: #f9fafb;
        }
        .tab-item.active:hover {
            background-color: #eff6ff;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .card {
            background: #fff;
            padding: 30px;
            border-radius: 0 0 12px 12px;
            box-shadow: 0 2px 15px rgba(0,0,0,0.08);
            margin-bottom: 20px;
        }
        .fixed-tip {
            font-size: 13px;
            color: #666;
            margin-bottom: 20px;
            padding-bottom: 12px;
            border-bottom: 1px solid #eee;
        }
        .form-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px 20px;
            margin-bottom: 24px;
        }
        .form-item.full {
            grid-column: 1 / -1;
        }
        .form-item label {
            display: block;
            font-size: 14px;
            margin-bottom: 6px;
            color: #333;
        }
        .required {
            color: #e53e3e;
            margin-left: 2px;
        }
        .form-item input {
            width: 100%;
            padding: 9px 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 14px;
            transition: border-color 0.2s;
        }
        .form-item input:focus {
            outline: none;
            border-color: #2563eb;
        }
        .btn-group {
            text-align: center;
            display: flex;
            gap: 12px;
            justify-content: center;
        }
        .btn {
            padding: 9px 28px;
            border: none;
            border-radius: 6px;
            font-size: 15px;
            cursor: pointer;
            transition: background 0.2s;
        }
        .btn-primary {
            background-color: #2563eb;
            color: #fff;
        }
        .btn-primary:hover {
            background-color: #1d4ed8;
        }
        .btn-default {
            background-color: #fff;
            border: 1px solid #ddd;
            color: #333;
        }
        .btn-default:hover {
            background-color: #f5f7fa;
        }
        .btn-danger {
            background-color: #e53e3e;
            color: #fff;
        }
        .btn-danger:hover {
            background-color: #dc2626;
        }
        .btn-sm {
            padding: 6px 14px;
            font-size: 13px;
        }
        .result-box {
            margin-top: 24px;
            padding: 18px;
            background-color: #dbeafe;
            color: #1e40af;
            border-radius: 8px;
            display: none;
        }
        .result-box p {
            margin: 6px 0;
            font-size: 15px;
        }
        .result-box span {
            font-weight: bold;
        }
        .history-head {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
            padding-bottom: 10px;
            border-bottom: 1px solid #eee;
        }
        .history-head h3 {
            font-size: 17px;
            color: #333;
            font-weight: 600;
        }
        .history-list {
            max-height: 400px;
            overflow-y: auto;
        }
        .record {
            padding: 14px 16px;
            border: 1px solid #eee;
            border-radius: 8px;
            margin-bottom: 12px;
            background: #fafafa;
            position: relative;
        }
        .record .del-btn {
            position: absolute;
            top: 12px;
            right: 14px;
            font-size: 12px;
            color: #999;
            cursor: pointer;
            padding: 3px 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            background: #fff;
            transition: all 0.2s;
        }
        .record .del-btn:hover {
            color: #e53e3e;
            border-color: #e53e3e;
        }
        .record .time {
            font-size: 12px;
            color: #888;
            margin-bottom: 8px;
            padding-right: 55px;
        }
        .record .row {
            display: flex;
            flex-wrap: wrap;
            gap: 16px;
            font-size: 13px;
            line-height: 1.8;
        }
        .record .row span {
            color: #666;
        }
        .record .row b {
            color: #333;
            font-weight: 500;
        }
        .empty {
            text-align: center;
            color: #999;
            padding: 30px 0;
            font-size: 14px;
        }
        .footer {
            text-align: center;
            color: #999;
            font-size: 12px;
            margin-top: 10px;
            padding-bottom: 20px;
        }
        @media (max-width: 600px) {
            .form-grid {
                grid-template-columns: 1fr;
            }
            .card {
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>涂布工艺参数计算器</h1>
        <div class="current-time" id="currentTime"></div>

        <!-- 切换菜单 -->
        <div class="tab-nav">
            <div class="tab-item active" onclick="switchTab(0)">涂布工艺计算</div>
            <div class="tab-item" onclick="switchTab(1)">膜长度计算</div>
        </div>

        <!-- 页面1：涂布工艺计算 -->
        <div class="tab-content active" id="tab0">
            <div class="card">
                <div class="fixed-tip">固定参数：树脂密度=2 | 溶液密度=1.102 | 系数=100</div>
                
                <div class="form-grid">
                    <div class="form-item">
                        <label>批号</label>
                        <input type="text" id="batchNo" placeholder="非必填">
                    </div>
                    <div class="form-item">
                        <label>粘度</label>
                        <input type="text" id="viscosity" placeholder="非必填">
                    </div>
                    <div class="form-item">
                        <label>固含量% <span class="required">*</span></label>
                        <input type="number" id="solidContent" placeholder="请输入">
                    </div>
                    <div class="form-item">
                        <label>速度m/min <span class="required">*</span></label>
                        <input type="number" id="speed" placeholder="请输入">
                    </div>
                    <div class="form-item">
                        <label>垫片宽度cm <span class="required">*</span></label>
                        <input type="number" id="gasketWidth" placeholder="请输入">
                    </div>
                    <div class="form-item">
                        <label>目标厚度μm <span class="required">*</span></label>
                        <input type="number" id="targetThickness" placeholder="请输入">
                    </div>
                    <div class="form-item full">
                        <label>备注/型号</label>
                        <input type="text" id="remark" placeholder="非必填">
                    </div>
                </div>

                <div class="btn-group">
                    <button class="btn btn-primary" onclick="calculate()">计算并保存</button>
                    <button class="btn btn-default" onclick="clearInput()">清空输入</button>
                </div>

                <div class="result-box" id="resultBox">
                    <p>蠕动泵流量：<span id="flowResult">-</span></p>
                    <p>液膜厚度：<span id="filmResult">-</span></p>
                </div>
            </div>

            <div class="card">
                <div class="history-head">
                    <h3>计算历史记录</h3>
                    <button class="btn btn-danger btn-sm" onclick="clearHistory()">清空历史</button>
                </div>
                <div class="history-list" id="historyList">
                    <div class="empty">暂无计算记录</div>
                </div>
            </div>
        </div>

        <!-- 页面2：膜长度计算 -->
        <div class="tab-content" id="tab1">
            <div class="card">
                <div class="fixed-tip">计算公式：π × (卷管直径×10 + 膜卷厚度) × 膜卷厚度 ÷ 单膜厚度</div>
                
                <div class="form-grid">
                    <div class="form-item">
                        <label>卷管外直径 cm</label>
                        <input type="number" id="tubeDiameter" value="18">
                    </div>
                    <div class="form-item">
                        <label>单膜厚度 μm</label>
                        <input type="number" id="filmThickness" value="125">
                    </div>
                    <div class="form-item full">
                        <label>膜卷厚度 mm <span class="required">*</span></label>
                        <input type="number" id="rollThickness" placeholder="请输入膜卷厚度">
                    </div>
                </div>

                <div class="btn-group">
                    <button class="btn btn-primary" onclick="calcFilmLength()">计算膜长度</button>
                    <button class="btn btn-default" onclick="clearFilmInput()">清空输入</button>
                </div>

                <div class="result-box" id="filmResultBox">
                    <p>膜总长度：<span id="filmLengthResult">-</span></p>
                </div>
            </div>
        </div>

        <div class="footer">
            © 海浪网络科技工作室 | 版本 V1.02
        </div>
    </div>

    <script>
        // 通用元素
        const els = {
            currentTime: document.getElementById('currentTime'),
            // 涂布计算元素
            batchNo: document.getElementById('batchNo'),
            viscosity: document.getElementById('viscosity'),
            solidContent: document.getElementById('solidContent'),
            speed: document.getElementById('speed'),
            gasketWidth: document.getElementById('gasketWidth'),
            targetThickness: document.getElementById('targetThickness'),
            remark: document.getElementById('remark'),
            flowResult: document.getElementById('flowResult'),
            filmResult: document.getElementById('filmResult'),
            resultBox: document.getElementById('resultBox'),
            historyList: document.getElementById('historyList'),
            // 膜长计算元素
            tubeDiameter: document.getElementById('tubeDiameter'),
            filmThickness: document.getElementById('filmThickness'),
            rollThickness: document.getElementById('rollThickness'),
            filmLengthResult: document.getElementById('filmLengthResult'),
            filmResultBox: document.getElementById('filmResultBox')
        };

        const CONST = { resinDensity: 2, solutionDensity: 1.102, coefficient: 100 };
        const STORAGE_KEY = 'coatingHistory';

        // 初始化
        window.onload = function() {
            updateCurrentTime();
            setInterval(updateCurrentTime, 60000);
            renderHistory();
        };

        // Tab切换
        function switchTab(index) {
            const tabs = document.querySelectorAll('.tab-item');
            const contents = document.querySelectorAll('.tab-content');
            tabs.forEach((t, i) => t.classList.toggle('active', i === index));
            contents.forEach((c, i) => c.classList.toggle('active', i === index));
        }

        // 更新当前时间
        function updateCurrentTime() {
            const now = new Date();
            const pad = n => String(n).padStart(2, '0');
            const timeStr = `${now.getFullYear()}年${pad(now.getMonth()+1)}月${pad(now.getDate())}日 ${pad(now.getHours())}:${pad(now.getMinutes())}`;
            els.currentTime.textContent = timeStr;
        }

        // ========== 涂布计算功能 ==========
        function calculate() {
            const data = getFormData();
            if (!validate(data)) return;

            const pumpFlow = (CONST.resinDensity * data.gasketWidth * data.speed * data.targetThickness) 
                            / (data.solidContent * CONST.solutionDensity);
            const filmThicknessVal = (pumpFlow * CONST.coefficient) / (data.speed * data.gasketWidth);

            els.flowResult.textContent = pumpFlow.toFixed(2);
            els.filmResult.textContent = filmThicknessVal.toFixed(2);
            els.resultBox.style.display = 'block';

            saveRecord({
                ...data,
                pumpFlow: pumpFlow.toFixed(2),
                filmThickness: filmThicknessVal.toFixed(2),
                saveTime: formatTime(new Date())
            });
            renderHistory();
        }

        function getFormData() {
            return {
                batchNo: els.batchNo.value.trim() || '-',
                viscosity: els.viscosity.value.trim() || '-',
                solidContent: parseFloat(els.solidContent.value),
                speed: parseFloat(els.speed.value),
                gasketWidth: parseFloat(els.gasketWidth.value),
                targetThickness: parseFloat(els.targetThickness.value),
                remark: els.remark.value.trim() || '-'
            };
        }

        function validate(data) {
            const required = ['solidContent', 'speed', 'gasketWidth', 'targetThickness'];
            for (const key of required) {
                if (isNaN(data[key]) || data[key] <= 0) {
                    alert('请填写所有必填项，且数值必须大于0');
                    return false;
                }
            }
            return true;
        }

        function clearInput() {
            ['batchNo','viscosity','solidContent','speed','gasketWidth','targetThickness','remark'].forEach(k => {
                els[k].value = '';
            });
            els.resultBox.style.display = 'none';
            els.solidContent.focus();
        }

        function getHistory() {
            try {
                return JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
            } catch (e) {
                return [];
            }
        }

        function saveRecord(record) {
            const history = getHistory();
            history.unshift(record);
            localStorage.setItem(STORAGE_KEY, JSON.stringify(history));
        }

        function deleteRecord(index) {
            if (!confirm('确定删除该条记录？')) return;
            const history = getHistory();
            history.splice(index, 1);
            localStorage.setItem(STORAGE_KEY, JSON.stringify(history));
            renderHistory();
        }

        function clearHistory() {
            if (!confirm('确定清空所有历史记录？此操作不可恢复。')) return;
            localStorage.removeItem(STORAGE_KEY);
            renderHistory();
        }

        function renderHistory() {
            const history = getHistory();
            if (history.length === 0) {
                els.historyList.innerHTML = '<div class="empty">暂无计算记录</div>';
                return;
            }

            els.historyList.innerHTML = history.map((item, idx) => `
                <div class="record">
                    <span class="del-btn" onclick="deleteRecord(${idx})">删除</span>
                    <div class="time">${item.saveTime}</div>
                    <div class="row">
                        <span>批号：<b>${item.batchNo}</b></span>
                        <span>粘度：<b>${item.viscosity}</b></span>
                    </div>
                    <div class="row">
                        <span>固含量：<b>${item.solidContent}%</b></span>
                        <span>速度：<b>${item.speed} m/min</b></span>
                        <span>垫片宽度：<b>${item.gasketWidth} cm</b></span>
                        <span>目标厚度：<b>${item.targetThickness} μm</b></span>
                    </div>
                    <div class="row">
                        <span>流量：<b>${item.pumpFlow}</b></span>
                        <span>液膜厚度：<b>${item.filmThickness}</b></span>
                        ${item.remark !== '-' ? `<span>备注：<b>${item.remark}</b></span>` : ''}
                    </div>
                </div>
            `).join('');
        }

        function formatTime(date) {
            const pad = n => String(n).padStart(2, '0');
            return `${date.getFullYear()}-${pad(date.getMonth()+1)}-${pad(date.getDate())} ${pad(date.getHours())}:${pad(date.getMinutes())}:${pad(date.getSeconds())}`;
        }

        // ========== 膜长度计算功能 ==========
        function calcFilmLength() {
            const tubeD = parseFloat(els.tubeDiameter.value);
            const filmT = parseFloat(els.filmThickness.value);
            const rollT = parseFloat(els.rollThickness.value);

            if (isNaN(tubeD) || isNaN(filmT) || isNaN(rollT) || tubeD <= 0 || filmT <= 0 || rollT <= 0) {
                alert('请填写所有参数，且数值必须大于0');
                return;
            }

            // 严格套用WPS公式：PI()*(B2*10+B4)*B4/B3
            const length = Math.PI * (tubeD * 10 + rollT) * rollT / filmT;
            
            els.filmLengthResult.textContent = length.toFixed(2);
            els.filmResultBox.style.display = 'block';
        }

        function clearFilmInput() {
            els.tubeDiameter.value = '18';
            els.filmThickness.value = '125';
            els.rollThickness.value = '';
            els.filmResultBox.style.display = 'none';
            els.rollThickness.focus();
        }
    </script>
</body>
</html>
