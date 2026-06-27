# Chemi-sim
محاكي المختبر الكيميائي
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>محاكاة تحضير بيركلورات الأمونيوم | مختبر كيميائي تفاعلي</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(145deg, #e0e7f0 0%, #f4f7fc 100%);
            font-family: 'Segoe UI', 'Tajawal', 'Cairo', system-ui, -apple-system, sans-serif;
            margin: 0;
            padding: 1.5rem;
            color: #1a2c3e;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* بطاقات */
        .card {
            background: rgba(255,255,255,0.92);
            backdrop-filter: blur(2px);
            border-radius: 2rem;
            box-shadow: 0 20px 35px -12px rgba(0,0,0,0.15), 0 0 0 1px rgba(156,180,204,0.2);
            padding: 1.5rem;
            margin-bottom: 2rem;
            transition: all 0.2s ease;
        }

        .card-header {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            border-bottom: 3px solid #2c7da0;
            padding-bottom: 0.75rem;
            margin-bottom: 1.5rem;
        }

        .card-header h2 {
            margin: 0;
            font-size: 1.6rem;
            color: #1f5068;
            letter-spacing: -0.3px;
        }

        .badge {
            background: #2c7da0;
            color: white;
            border-radius: 60px;
            padding: 0.25rem 1rem;
            font-size: 0.8rem;
            font-weight: bold;
        }

        .grid-2col {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 1.8rem;
        }

        /* الجداول والقوائم */
        .info-table {
            width: 100%;
            border-collapse: collapse;
            background: #fefefe;
            border-radius: 1.2rem;
            overflow: hidden;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }
        .info-table td, .info-table th {
            padding: 0.8rem 1rem;
            border-bottom: 1px solid #e2edf2;
        }
        .info-table tr:last-child td {
            border-bottom: none;
        }
        .info-table th {
            background: #eef3f8;
            font-weight: 600;
            width: 40%;
        }

        .param-input {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 0.8rem;
            margin-bottom: 1rem;
            background: #f8fafd;
            padding: 0.8rem;
            border-radius: 1.2rem;
        }
        .param-input label {
            font-weight: bold;
            width: 140px;
        }
        .param-input input {
            flex: 1;
            padding: 0.6rem 1rem;
            border: 1px solid #cbdde6;
            border-radius: 2rem;
            font-size: 1rem;
            background: white;
            font-family: monospace;
        }
        button {
            background: #2c7da0;
            border: none;
            color: white;
            padding: 0.6rem 1.4rem;
            border-radius: 2rem;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
            font-size: 0.9rem;
        }
        button:hover {
            background: #1f5e7a;
            transform: scale(0.97);
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        .result-box {
            background: #eef3fc;
            border-radius: 1rem;
            padding: 1rem;
            margin-top: 1rem;
            font-weight: 500;
            font-family: monospace;
            font-size: 1rem;
            border-right: 5px solid #2c7da0;
        }
        .step-list {
            list-style-type: none;
            padding: 0;
        }
        .step-list li {
            padding: 0.5rem 0 0.5rem 1.5rem;
            margin: 0.5rem 0;
            background: #f9fbfd;
            border-radius: 1rem;
            position: relative;
        }
        .step-list li::before {
            content: "⚗️";
            position: absolute;
            right: -0.2rem;
            top: 0.5rem;
            font-size: 1rem;
        }
        hr {
            margin: 1.5rem 0;
            border-color: #cde1ec;
        }
        .speed-sim {
            background: #1a2c3e0c;
            border-radius: 1.2rem;
            padding: 1rem;
        }
        @media (max-width: 700px) {
            body { padding: 1rem; }
            .card-header h2 { font-size: 1.3rem; }
        }
        footer {
            text-align: center;
            font-size: 0.75rem;
            color: #4a627a;
            margin-top: 2rem;
        }
        .warning-badge {
            background: #f4d03f;
            color: #7d5d00;
            border-radius: 2rem;
            padding: 0.2rem 0.8rem;
            font-size: 0.7rem;
        }
    </style>
</head>
<body>
<div class="container">
    <!-- عنوان رئيسي -->
    <div class="card" style="background: #1f5068; color: white; text-align: center;">
        <h1 style="margin:0; font-size:1.8rem;">🧪 محاكاة تفاعلية: تحضير بيركلورات الأمونيوم (AP)</h1>
        <p style="margin-top:0.5rem; opacity:0.9;">NaClO₄·H₂O + NH₄Cl → NH₄ClO₄ + NaCl | تصميم مستند إلى بيانات حقيقية ومخطط المحادثة</p>
        <div class="warning-badge" style="background:#ffd966; color:#6b4c00; display:inline-block;">⚠️ تحذير: مؤكسد قوي – تطبيق بروتوكولات الأمان إلزامي</div>
    </div>

    <!-- لوحة التحكم الرئيسية: المدخلات والحسابات -->
    <div class="card">
        <div class="card-header">
            <h2>📊 مدخلات العملية (مقياس تجريبي)</h2>
            <div class="badge">تفاعل تبادل مزدوج</div>
        </div>
        <div class="grid-2col">
            <div>
                <div class="param-input">
                    <label>🧂 NH₄Cl (تجاري) :</label>
                    <input type="number" id="nh4cl_mass" value="17" step="0.5"> <span>كجم</span>
                </div>
                <div class="param-input">
                    <label>🧪 نقاوة NH₄Cl :</label>
                    <input type="number" id="purity_nh4cl" value="99.5" step="0.1"> <span>%</span>
                </div>
                <div class="param-input">
                    <label>⚪ NaClO₄·H₂O نقاوة :</label>
                    <input type="number" id="purity_naclo4" value="98.5" step="0.1"> <span>%</span>
                </div>
                <div class="param-input">
                    <label>💧 رطوبة NaClO₄·H₂O :</label>
                    <input type="number" id="moisture" value="13" step="0.5"> <span>% (تطابق ماء التبلور)</span>
                </div>
                <button id="calcBtn">🔄 إعادة حساب الكميات والماء</button>
            </div>
            <div>
                <div class="result-box" id="summaryResult">
                    <strong>📌 النتائج الأساسية (استناداً إلى 17 كجم NH₄Cl):</strong><br>
                    • كلوريد الأمونيوم النقي: <span id="pure_nh4cl">--</span> كجم<br>
                    • المولات: <span id="moles_nh4">--</span> مول<br>
                    • بيركلورات الصوديوم المطلوب (نقي): <span id="req_naclo4_pure">--</span> كجم<br>
                    • الكتلة التجارية (NaClO₄·H₂O): <span id="commercial_naclo4">--</span> كجم<br>
                    • الناتج النظري NH₄ClO₄: <span id="theo_ap">--</span> كجم<br>
                    • 💧 حجم الماء الأمثل (لسخان 85-90°م): <span id="water_opt">--</span> لتر<br>
                    • 🧊 المردود المتوقع بعد التبريد (0-5°م): ≈ <span id="yield_est">--</span> كجم
                </div>
            </div>
        </div>
    </div>

    <!-- مخطط الخطوات + سرعة الخلاط والإنفرتر -->
    <div class="grid-2col">
        <div class="card">
            <div class="card-header">
                <h2>📋 سير العملية (التسخين/التبريد)</h2>
                <div class="badge">بروتوكول متكامل</div>
            </div>
            <ul class="step-list">
                <li>تسخين 55-58 لتر ماء إلى 85-90°م (حسب الحساب أعلاه).</li>
                <li>إضافة NaClO₄·H₂O (45.1 كجم نموذجياً) + NH₄Cl (17 كجم) مع تحريك 200-400 دورة/دقيقة.</li>
                <li>التبريد البطيء إلى 55-65°م، ثم إضافة البذرة (50-100 جم بحجم ~50 ميكرون).</li>
                <li>مواصلة التبريد الطبيعي إلى 25°م (1-2 ساعة)، تحريك 50-100 دورة/دقيقة.</li>
                <li>التبريد في حمام جليدي (0-5°م) لمدة 1-1.5 ساعة (يُفضل إيقاف التحريك).</li>
                <li>ترشيح وغسل بلورات AP بماء مثلج (10-15 لتر).</li>
                <li>تجفيف تحت تفريغ عند ≤50°م أو في مجفف P₂O₅.</li>
            </ul>
            <div class="warning-badge" style="background:#f9e0a0;">⭐ ملاحظة: زمن التبلور الكلي ~ 3-4 ساعات + تجفيف</div>
        </div>

        <!-- جهاز التحكم في سرعة الخلاط والتردد (محاكاة الإنفرتر) -->
        <div class="card">
            <div class="card-header">
                <h2>⚙️ محاكاة سرعة الخلاط + الإنفرتر</h2>
                <div class="badge">معطيات المحرك: 2800 rpm عند 50Hz | مخفض 1:5.8</div>
            </div>
            <div class="speed-sim">
                <div class="param-input">
                    <label>🎛️ التردد المطلوب (هيرتز):</label>
                    <input type="number" id="hz_input" value="50" step="1">
                    <button id="calcRpm">احسب سرعة الخرج</button>
                </div>
                <div class="result-box" id="rpmResult">
                    🔹 <strong>سرعة الخرج (بعد المخفض):</strong> <span id="outputRpm">--</span> دورة/دقيقة<br>
                    🔹 <strong>سرعة المحرك الأساسية:</strong> <span id="motorRpm">--</span> rpm
                </div>
                <hr>
                <div class="param-input">
                    <label>🎯 السرعة المستهدفة عند الخرج (rpm):</label>
                    <input type="number" id="targetRpm" value="200" step="10">
                    <button id="calcHzFromRpm">احسب التردد المطلوب</button>
                </div>
                <div class="result-box" id="hzResult">
                    📡 <strong>التردد اللازم:</strong> <span id="requiredHz">--</span> هيرتز
                </div>
                <div style="font-size:0.85rem; margin-top:1rem; background:#eef1f5; border-radius:1rem; padding:0.6rem;">
                    💡 تلميح: مرحلة التسخين تحتاج 200-400 rpm ← تحتاج تردد ~20.7 إلى 41.4 هيرتز. مرحلة التبريد المنخفضة ~50-100 rpm تحتاج 5-10 هيرتز (تحقق من تبريد المحرك).
                </div>
            </div>
        </div>
    </div>

    <!-- قسم خاص لحجم البذرة والمردود وحجم البلورة النهائي -->
    <div class="card">
        <div class="card-header">
            <h2>🌱 حساب البذرة وحجم البلورة النهائي (355 ميكرون)</h2>
            <div class="badge">معادلة النمو m_f/m_s = (L_f/L_s)³</div>
        </div>
        <div class="grid-2col">
            <div>
                <div class="param-input">
                    <label>📦 كتلة البذور المضافة (جم):</label>
                    <input type="number" id="seed_mass_g" value="100" step="10">
                    <button id="calcSeedSize">حجم البذرة المناسب</button>
                </div>
                <div class="result-box">
                    🧬 <strong>حجم البذرة المطلوب (L_seed)</strong> لبلورة نهائية 355 ميكرون:<br>
                    <span id="seedSizeResult">--</span> ميكرون
                </div>
            </div>
            <div>
                <div class="param-input">
                    <label>🧪 حجم البذرة المدخلة (ميكرون):</label>
                    <input type="number" id="input_seed_size" value="55" step="5">
                    <button id="calcFinalCrystal">احسب حجم البلورة الناتجة</button>
                </div>
                <div class="result-box">
                    🔬 <strong>حجم البلورة النهائي المتوقع:</strong> <span id="finalCrystalSize">--</span> ميكرون
                </div>
            </div>
        </div>
        <div class="warning-badge" style="background:#e6f4ea;">⚠️ الحسابات تقريبية وتعتمد على عدم وجود تبلور ثانوي. التحكم بسرعة التبريد يؤثر على النتيجة.</div>
    </div>

    <footer>
        واجهة محاكاة مستندة إلى المحادثة الهندسية الكاملة | بيانات تفاعلية - التطبيق للأغراض التعليمية والتخطيط المخبري
    </footer>
</div>

<script>
    // المعطيات الثابتة للتفاعل
    const MW_NH4Cl = 53.5;      // g/mol
    const MW_NaClO4_H2O = 140.5; // g/mol
    const MW_AP = 117.5;         // NH4ClO4 g/mol
    const SOL_AP_HOT = 700;      // g/L عند 80°C (تقدير تقريبي)
    const SOL_AP_COLD = 150;     // g/L عند 0-5°C (ذوبانية AP)

    // دوال مساعدة للحسابات الأساسية
    function computeMainParams() {
        let nh4cl_kg = parseFloat(document.getElementById('nh4cl_mass').value);
        let purity_nh4 = parseFloat(document.getElementById('purity_nh4cl').value) / 100;
        let purity_naclo4 = parseFloat(document.getElementById('purity_naclo4').value) / 100;
        // الرطوبة لا تؤثر على الوزن الجزيئي النظري لأنها مطابقة لماء التبلور (نستخدم الصيغة التجارية)
        
        let nh4cl_pure_kg = nh4cl_kg * purity_nh4;
        let moles_nh4 = (nh4cl_pure_kg * 1000) / MW_NH4Cl;
        
        // كتلة NaClO4·H2O النقية المطلوبة (نظرية)
        let naclo4_pure_kg = (moles_nh4 * MW_NaClO4_H2O) / 1000;
        let naclo4_commercial_kg = naclo4_pure_kg / purity_naclo4;
        
        // الناتج النظري NH4ClO4 بالكجم
        let ap_theoretical_kg = (moles_nh4 * MW_AP) / 1000;
        
        // حساب حجم الماء الأمثل: هدفنا إذابة AP الساخن بالكامل (ذوبانية 700 جم/لتر عند 80°C)
        // الماء اللازم لضمان عدم ترسب AP أثناء التسخين = (كتلة AP الناتجة بالجرام) / 700 
        let water_liters_min = (ap_theoretical_kg * 1000) / SOL_AP_HOT;
        // إضافة هامش أمان 5-10% وإذابة NaCl أيضاً لكن العامل المحدد AP
        let water_opt = water_liters_min * 1.08; // 8% زيادة عملية
        water_opt = Math.ceil(water_opt * 10) / 10;
        
        // تقدير الفاقد والمردود بعد التبريد (المحلول المتبقي يحوي AP ذائب عند 0-5°C)
        let ap_dissolved_cold_kg = (water_opt * SOL_AP_COLD) / 1000;
        let ap_yield_kg = Math.max(0, ap_theoretical_kg - ap_dissolved_cold_kg);
        ap_yield_kg = Math.round(ap_yield_kg * 10) / 10;
        
        // تحديث الواجهة
        document.getElementById('pure_nh4cl').innerText = nh4cl_pure_kg.toFixed(2);
        document.getElementById('moles_nh4').innerText = moles_nh4.toFixed(1);
        document.getElementById('req_naclo4_pure').innerText = naclo4_pure_kg.toFixed(2);
        document.getElementById('commercial_naclo4').innerText = naclo4_commercial_kg.toFixed(2);
        document.getElementById('theo_ap').innerText = ap_theoretical_kg.toFixed(2);
        document.getElementById('water_opt').innerText = water_opt.toFixed(1) + " لتر";
        document.getElementById('yield_est').innerHTML = ap_yield_kg.toFixed(1) + " كجم (مردود ~" + ((ap_yield_kg/ap_theoretical_kg)*100).toFixed(0) + "%)";
        
        // إرجاع القيم للاستخدام في دوال البذرة
        return { ap_theoretical_kg, water_opt, moles_nh4, naclo4_commercial_kg };
    }
    
    // حساب علاقة البذور: Lf/Ls = (mf/ms)^(1/3)
    function computeSeedSize() {
        let ap_theo = computeMainParams().ap_theoretical_kg;
        let m_final_kg = ap_theo;   // كتلة المنتج الكلية (نفترض أن الكتلة الكلية للناتج النظري تقريباً تنمو من البذور)
        let seed_g = parseFloat(document.getElementById('seed_mass_g').value);
        if(isNaN(seed_g) || seed_g <= 0) seed_g = 100;
        let m_seed_kg = seed_g / 1000;
        let ratio_m = m_final_kg / m_seed_kg;
        let Lf = 355; // ميكرون المطلوب
        let Ls = Lf / Math.pow(ratio_m, 1/3);
        if(!isFinite(Ls)) Ls = 0;
        document.getElementById('seedSizeResult').innerHTML = Ls.toFixed(1) + " ميكرون (≈ " + Math.round(Ls) + " µm)";
    }
    
    function computeFinalCrystal() {
        let ap_theo = computeMainParams().ap_theoretical_kg;
        let seed_g = parseFloat(document.getElementById('seed_mass_g').value);
        if(isNaN(seed_g)) seed_g = 100;
        let m_seed_kg = seed_g / 1000;
        let ratio_m = ap_theo / m_seed_kg;
        let Ls_input = parseFloat(document.getElementById('input_seed_size').value);
        if(isNaN(Ls_input) || Ls_input <= 0) Ls_input = 55;
        let L_final = Ls_input * Math.pow(ratio_m, 1/3);
        document.getElementById('finalCrystalSize').innerHTML = L_final.toFixed(1) + " ميكرون";
    }
    
    // --- دوال الإنفرتر والمحرك ---
    const MOTOR_BASE_RPM = 2800;    // عند 50Hz
    const GEAR_RATIO = 5.8;         // المخفض 1:5.8
    function calcOutputRpmFromHz(hz) {
        let motorRpm = (MOTOR_BASE_RPM / 50) * hz; // علاقة خطية
        let outputRpm = motorRpm / GEAR_RATIO;
        return { motorRpm, outputRpm };
    }
    function calcHzFromOutputRpm(targetRpm) {
        let motorRpmNeeded = targetRpm * GEAR_RATIO;
        let hz = (motorRpmNeeded / MOTOR_BASE_RPM) * 50;
        return hz;
    }
    
    function updateRpmFromHz() {
        let hz = parseFloat(document.getElementById('hz_input').value);
        if(isNaN(hz)) hz = 50;
        let { motorRpm, outputRpm } = calcOutputRpmFromHz(hz);
        document.getElementById('motorRpm').innerText = Math.round(motorRpm) + " rpm";
        document.getElementById('outputRpm').innerText = Math.round(outputRpm) + " rpm";
    }
    function updateHzFromTarget() {
        let target = parseFloat(document.getElementById('targetRpm').value);
        if(isNaN(target)) target = 200;
        let hzNeeded = calcHzFromOutputRpm(target);
        document.getElementById('requiredHz').innerHTML = hzNeeded.toFixed(1) + " هيرتز (≈ " + Math.round(hzNeeded) + " Hz)";
    }
    
    // ربط الأحداث
    document.getElementById('calcBtn').addEventListener('click', () => {
        computeMainParams();
        computeSeedSize();   // تحديث تابع للبذرة بعد إعادة الحساب
        computeFinalCrystal();
    });
    document.getElementById('calcSeedSize').addEventListener('click', computeSeedSize);
    document.getElementById('calcFinalCrystal').addEventListener('click', computeFinalCrystal);
    document.getElementById('calcRpm').addEventListener('click', updateRpmFromHz);
    document.getElementById('calcHzFromRpm').addEventListener('click', updateHzFromTarget);
    
    // تشغيل أولي
    window.addEventListener('DOMContentLoaded', () => {
        computeMainParams();
        computeSeedSize();
        computeFinalCrystal();
        updateRpmFromHz();
        updateHzFromTarget();
    });
</script>
</body>
</html>

