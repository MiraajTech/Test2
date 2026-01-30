@ -0,0 +1,1058 @@
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pharmacy Dosage Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        header {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            margin-bottom: 30px;
            text-align: center;
        }

        h1 {
            color: #1e3c72;
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        .subtitle {
            color: #666;
            font-size: 1.1em;
        }

        .calculator-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
            gap: 25px;
            margin-bottom: 30px;
        }

        .calculator-card {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .calculator-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 40px rgba(0,0,0,0.3);
        }

        .calculator-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 3px solid #1e3c72;
        }

        .calculator-icon {
            font-size: 2.5em;
            margin-right: 15px;
        }

        .calculator-title {
            font-size: 1.5em;
            color: #1e3c72;
            font-weight: bold;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: 600;
            font-size: 0.95em;
        }

        input, select {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 1em;
            transition: border-color 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #1e3c72;
        }

        input[type="number"] {
            -moz-appearance: textfield;
        }

        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        .input-group {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 10px;
        }

        .btn {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: white;
            border: none;
            padding: 14px 25px;
            border-radius: 8px;
            font-size: 1em;
            cursor: pointer;
            width: 100%;
            transition: transform 0.2s, box-shadow 0.2s;
            font-weight: 600;
            margin-top: 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(30, 60, 114, 0.4);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn-clear {
            background: #6c757d;
            margin-top: 10px;
        }

        .result-box {
            background: #e8f5e9;
            border-left: 4px solid #4caf50;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
            display: none;
        }

        .result-box.show {
            display: block;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .result-title {
            font-size: 1.1em;
            font-weight: bold;
            color: #2e7d32;
            margin-bottom: 10px;
        }

        .result-value {
            font-size: 1.8em;
            font-weight: bold;
            color: #1b5e20;
            margin-bottom: 10px;
        }

        .result-details {
            color: #555;
            font-size: 0.95em;
            line-height: 1.6;
        }

        .warning-box {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            color: #856404;
        }

        .warning-title {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .info-box {
            background: #e3f2fd;
            border-left: 4px solid #2196f3;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            color: #0d47a1;
            font-size: 0.9em;
        }

        .formula-box {
            background: #f5f5f5;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            font-family: 'Courier New', monospace;
            font-size: 0.9em;
            color: #333;
        }

        .quick-ref {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            margin-top: 30px;
        }

        .quick-ref h2 {
            color: #1e3c72;
            margin-bottom: 20px;
            font-size: 1.8em;
        }

        .ref-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }

        .ref-card {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #1e3c72;
        }

        .ref-title {
            font-weight: bold;
            color: #1e3c72;
            margin-bottom: 8px;
        }

        .ref-content {
            color: #555;
            font-size: 0.9em;
            line-height: 1.6;
        }

        @media (max-width: 768px) {
            .calculator-grid {
                grid-template-columns: 1fr;
            }

            h1 {
                font-size: 2em;
            }

            .input-group {
                grid-template-columns: 1fr;
            }
        }

        .toggle-formula {
            color: #1e3c72;
            cursor: pointer;
            text-decoration: underline;
            font-size: 0.9em;
            margin-top: 10px;
            display: inline-block;
        }

        .toggle-formula:hover {
            color: #2a5298;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🧮 Pharmacy Dosage Calculator</h1>
            <p class="subtitle">Comprehensive calculation tools for pharmacy professionals</p>
        </header>

        <div class="calculator-grid">
            <!-- Pediatric Weight-Based Dosing -->
            <div class="calculator-card">
                <div class="calculator-header">
                    <div class="calculator-icon">👶</div>
                    <div class="calculator-title">Pediatric Dose Calculator</div>
                </div>

                <div class="form-group">
                    <label>Patient Weight</label>
                    <div class="input-group">
                        <input type="number" id="ped-weight" placeholder="Enter weight" step="0.1">
                        <select id="ped-weight-unit">
                            <option value="kg">kg</option>
                            <option value="lbs">lbs</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Dose per kg</label>
                    <div class="input-group">
                        <input type="number" id="ped-dose-per-kg" placeholder="e.g., 10" step="0.1">
                        <select id="ped-dose-unit">
                            <option value="mg">mg/kg</option>
                            <option value="mcg">mcg/kg</option>
                            <option value="units">units/kg</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Frequency per Day</label>
                    <input type="number" id="ped-frequency" placeholder="e.g., 3" min="1" max="24">
                </div>

                <div class="form-group">
                    <label>Maximum Single Dose (optional)</label>
                    <input type="number" id="ped-max-dose" placeholder="Leave blank if no max" step="0.1">
                </div>

                <button class="btn" onclick="calculatePediatricDose()">Calculate Dose</button>
                <button class="btn btn-clear" onclick="clearPediatric()">Clear</button>

                <div id="ped-result" class="result-box"></div>
            </div>

            <!-- IV Drip Rate Calculator -->
            <div class="calculator-card">
                <div class="calculator-header">
                    <div class="calculator-icon">💉</div>
                    <div class="calculator-title">IV Drip Rate Calculator</div>
                </div>

                <div class="form-group">
                    <label>Volume to Infuse</label>
                    <div class="input-group">
                        <input type="number" id="iv-volume" placeholder="Enter volume" step="0.1">
                        <select id="iv-volume-unit">
                            <option value="ml">mL</option>
                            <option value="L">L</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Infusion Time</label>
                    <div class="input-group">
                        <input type="number" id="iv-time" placeholder="Enter time" step="0.1">
                        <select id="iv-time-unit">
                            <option value="hours">Hours</option>
                            <option value="minutes">Minutes</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Drop Factor (drops/mL)</label>
                    <select id="iv-drop-factor">
                        <option value="10">10 (Macrodrip)</option>
                        <option value="15">15 (Macrodrip)</option>
                        <option value="20">20 (Macrodrip)</option>
                        <option value="60">60 (Microdrip)</option>
                    </select>
                </div>

                <button class="btn" onclick="calculateIVRate()">Calculate Rate</button>
                <button class="btn btn-clear" onclick="clearIV()">Clear</button>

                <div id="iv-result" class="result-box"></div>
            </div>

            <!-- BSA Calculator (Mosteller Formula) -->
            <div class="calculator-card">
                <div class="calculator-header">
                    <div class="calculator-icon">📏</div>
                    <div class="calculator-title">Body Surface Area (BSA)</div>
                </div>

                <div class="form-group">
                    <label>Height</label>
                    <div class="input-group">
                        <input type="number" id="bsa-height" placeholder="Enter height" step="0.1">
                        <select id="bsa-height-unit">
                            <option value="cm">cm</option>
                            <option value="inches">inches</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Weight</label>
                    <div class="input-group">
                        <input type="number" id="bsa-weight" placeholder="Enter weight" step="0.1">
                        <select id="bsa-weight-unit">
                            <option value="kg">kg</option>
                            <option value="lbs">lbs</option>
                        </select>
                    </div>
                </div>

                <button class="btn" onclick="calculateBSA()">Calculate BSA</button>
                <button class="btn btn-clear" onclick="clearBSA()">Clear</button>

                <div id="bsa-result" class="result-box"></div>
            </div>

            <!-- BMI Calculator -->
            <div class="calculator-card">
                <div class="calculator-header">
                    <div class="calculator-icon">⚖️</div>
                    <div class="calculator-title">BMI Calculator</div>
                </div>

                <div class="form-group">
                    <label>Height</label>
                    <div class="input-group">
                        <input type="number" id="bmi-height" placeholder="Enter height" step="0.1">
                        <select id="bmi-height-unit">
                            <option value="cm">cm</option>
                            <option value="inches">inches</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Weight</label>
                    <div class="input-group">
                        <input type="number" id="bmi-weight" placeholder="Enter weight" step="0.1">
                        <select id="bmi-weight-unit">
                            <option value="kg">kg</option>
                            <option value="lbs">lbs</option>
                        </select>
                    </div>
                </div>

                <button class="btn" onclick="calculateBMI()">Calculate BMI</button>
                <button class="btn btn-clear" onclick="clearBMI()">Clear</button>

                <div id="bmi-result" class="result-box"></div>
            </div>

            <!-- Creatinine Clearance (Cockcroft-Gault) -->
            <div class="calculator-card">
                <div class="calculator-header">
                    <div class="calculator-icon">🩺</div>
                    <div class="calculator-title">Creatinine Clearance</div>
                </div>

                <div class="form-group">
                    <label>Age (years)</label>
                    <input type="number" id="crcl-age" placeholder="Enter age" min="1" max="120">
                </div>

                <div class="form-group">
                    <label>Weight (kg)</label>
                    <input type="number" id="crcl-weight" placeholder="Enter weight" step="0.1">
                </div>

                <div class="form-group">
                    <label>Serum Creatinine</label>
                    <div class="input-group">
                        <input type="number" id="crcl-scr" placeholder="Enter SCr" step="0.01">
                        <select id="crcl-scr-unit">
                            <option value="mg/dL">mg/dL</option>
                            <option value="µmol/L">µmol/L</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Sex</label>
                    <select id="crcl-sex">
                        <option value="male">Male</option>
                        <option value="female">Female</option>
                    </select>
                </div>

                <button class="btn" onclick="calculateCrCl()">Calculate CrCl</button>
                <button class="btn btn-clear" onclick="clearCrCl()">Clear</button>

                <div id="crcl-result" class="result-box"></div>
            </div>

            <!-- Dilution Calculator -->
            <div class="calculator-card">
                <div class="calculator-header">
                    <div class="calculator-icon">🧪</div>
                    <div class="calculator-title">Dilution Calculator</div>
                </div>

                <div class="form-group">
                    <label>Stock Concentration</label>
                    <div class="input-group">
                        <input type="number" id="dil-stock-conc" placeholder="e.g., 1000" step="0.1">
                        <select id="dil-stock-unit">
                            <option value="mg/mL">mg/mL</option>
                            <option value="mcg/mL">mcg/mL</option>
                            <option value="%">%</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Desired Concentration</label>
                    <div class="input-group">
                        <input type="number" id="dil-desired-conc" placeholder="e.g., 100" step="0.1">
                        <select id="dil-desired-unit">
                            <option value="mg/mL">mg/mL</option>
                            <option value="mcg/mL">mcg/mL</option>
                            <option value="%">%</option>
                        </select>
                    </div>
                </div>

                <div class="form-group">
                    <label>Final Volume Needed (mL)</label>
                    <input type="number" id="dil-final-volume" placeholder="e.g., 100" step="0.1">
                </div>

                <button class="btn" onclick="calculateDilution()">Calculate Dilution</button>
                <button class="btn btn-clear" onclick="clearDilution()">Clear</button>

                <div id="dil-result" class="result-box"></div>
            </div>
        </div>

        <!-- Quick Reference Guide -->
        <div class="quick-ref">
            <h2>📚 Quick Reference Guide</h2>
            <div class="ref-grid">
                <div class="ref-card">
                    <div class="ref-title">Common Pediatric Doses</div>
                    <div class="ref-content">
                        <strong>Paracetamol:</strong> 10-15 mg/kg every 4-6 hours<br>
                        <strong>Ibuprofen:</strong> 5-10 mg/kg every 6-8 hours<br>
                        <strong>Amoxicillin:</strong> 20-40 mg/kg/day divided 8-hourly
                    </div>
                </div>

                <div class="ref-card">
                    <div class="ref-title">IV Drop Factors</div>
                    <div class="ref-content">
                        <strong>Macrodrip:</strong> 10, 15, or 20 drops/mL<br>
                        <strong>Microdrip:</strong> 60 drops/mL<br>
                        <strong>Blood sets:</strong> Usually 15 drops/mL
                    </div>
                </div>

                <div class="ref-card">
                    <div class="ref-title">BSA Normal Range</div>
                    <div class="ref-content">
                        <strong>Average adult:</strong> 1.7 m²<br>
                        <strong>Range:</strong> 1.5 - 2.0 m²<br>
                        Used for chemotherapy and advanced dosing
                    </div>
                </div>

                <div class="ref-card">
                    <div class="ref-title">BMI Categories</div>
                    <div class="ref-content">
                        <strong>Underweight:</strong> &lt; 18.5<br>
                        <strong>Normal:</strong> 18.5 - 24.9<br>
                        <strong>Overweight:</strong> 25 - 29.9<br>
                        <strong>Obese:</strong> ≥ 30
                    </div>
                </div>

                <div class="ref-card">
                    <div class="ref-title">Renal Function (CrCl)</div>
                    <div class="ref-content">
                        <strong>Normal:</strong> 90-120 mL/min<br>
                        <strong>Mild impairment:</strong> 60-89<br>
                        <strong>Moderate:</strong> 30-59<br>
                        <strong>Severe:</strong> &lt; 30
                    </div>
                </div>

                <div class="ref-card">
                    <div class="ref-title">Unit Conversions</div>
                    <div class="ref-content">
                        <strong>1 kg</strong> = 2.205 lbs<br>
                        <strong>1 inch</strong> = 2.54 cm<br>
                        <strong>1 mg</strong> = 1000 mcg<br>
                        <strong>1 L</strong> = 1000 mL
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Utility Functions
        function showResult(elementId, content) {
            const element = document.getElementById(elementId);
            element.innerHTML = content;
            element.classList.add('show');
        }

        function hideResult(elementId) {
            const element = document.getElementById(elementId);
            element.classList.remove('show');
        }

        // Unit Conversion Functions
        function kgToLbs(kg) {
            return kg * 2.20462;
        }

        function lbsToKg(lbs) {
            return lbs / 2.20462;
        }

        function cmToInches(cm) {
            return cm / 2.54;
        }

        function inchesToCm(inches) {
            return inches * 2.54;
        }

        function umolToMg(umol) {
            return umol / 88.42;
        }

        // 1. Pediatric Dose Calculator
        function calculatePediatricDose() {
            const weight = parseFloat(document.getElementById('ped-weight').value);
            const weightUnit = document.getElementById('ped-weight-unit').value;
            const dosePerKg = parseFloat(document.getElementById('ped-dose-per-kg').value);
            const doseUnit = document.getElementById('ped-dose-unit').value;
            const frequency = parseFloat(document.getElementById('ped-frequency').value);
            const maxDose = parseFloat(document.getElementById('ped-max-dose').value);

            if (!weight || !dosePerKg || !frequency) {
                alert('Please fill in all required fields');
                return;
            }

            // Convert weight to kg if needed
            const weightKg = weightUnit === 'lbs' ? lbsToKg(weight) : weight;

            // Calculate single dose
            let singleDose = weightKg * dosePerKg;
            const unit = doseUnit.split('/')[0]; // Extract mg, mcg, or units

            // Check against max dose
            let maxDoseWarning = '';
            if (maxDose && singleDose > maxDose) {
                maxDoseWarning = `
                    <div class="warning-box">
                        <div class="warning-title">⚠️ Maximum Dose Exceeded</div>
                        Calculated dose (${singleDose.toFixed(2)} ${unit}) exceeds maximum single dose (${maxDose} ${unit}).
                        <br><strong>Using maximum dose instead.</strong>
                    </div>
                `;
                singleDose = maxDose;
            }

            const dailyDose = singleDose * frequency;

            const result = `
                <div class="result-title">Calculated Doses</div>
                <div class="result-value">${singleDose.toFixed(2)} ${unit} per dose</div>
                <div class="result-details">
                    <strong>Patient Weight:</strong> ${weightKg.toFixed(2)} kg<br>
                    <strong>Prescribed Dose:</strong> ${dosePerKg} ${doseUnit}<br>
                    <strong>Single Dose:</strong> ${singleDose.toFixed(2)} ${unit}<br>
                    <strong>Frequency:</strong> ${frequency} times per day<br>
                    <strong>Total Daily Dose:</strong> ${dailyDose.toFixed(2)} ${unit}/day
                </div>
                ${maxDoseWarning}
                <div class="formula-box" id="ped-formula" style="display:none;">
                    <strong>Formula:</strong><br>
                    Single Dose = Weight (kg) × Dose per kg<br>
                    Single Dose = ${weightKg.toFixed(2)} × ${dosePerKg} = ${singleDose.toFixed(2)} ${unit}<br>
                    Daily Dose = Single Dose × Frequency
                </div>
                <a class="toggle-formula" onclick="toggleFormula('ped-formula')">Show Formula</a>
            `;

            showResult('ped-result', result);
        }

        function clearPediatric() {
            document.getElementById('ped-weight').value = '';
            document.getElementById('ped-dose-per-kg').value = '';
            document.getElementById('ped-frequency').value = '';
            document.getElementById('ped-max-dose').value = '';
            hideResult('ped-result');
        }

        // 2. IV Drip Rate Calculator
        function calculateIVRate() {
            let volume = parseFloat(document.getElementById('iv-volume').value);
            const volumeUnit = document.getElementById('iv-volume-unit').value;
            let time = parseFloat(document.getElementById('iv-time').value);
            const timeUnit = document.getElementById('iv-time-unit').value;
            const dropFactor = parseFloat(document.getElementById('iv-drop-factor').value);

            if (!volume || !time) {
                alert('Please fill in all required fields');
                return;
            }

            // Convert to mL
            if (volumeUnit === 'L') {
                volume = volume * 1000;
            }

            // Convert to hours
            if (timeUnit === 'minutes') {
                time = time / 60;
            }

            // Calculate rates
            const mlPerHour = volume / time;
            const dropsPerMin = (volume * dropFactor) / (time * 60);

            const result = `
                <div class="result-title">Infusion Rates</div>
                <div class="result-value">${Math.round(dropsPerMin)} drops/min</div>
                <div class="result-details">
                    <strong>Volume:</strong> ${volume} mL<br>
                    <strong>Time:</strong> ${time.toFixed(2)} hours<br>
                    <strong>Drop Factor:</strong> ${dropFactor} drops/mL<br>
                    <strong>Flow Rate:</strong> ${mlPerHour.toFixed(1)} mL/hour<br>
                    <strong>Drip Rate:</strong> ${Math.round(dropsPerMin)} drops/minute
                </div>
                <div class="info-box">
                    💡 <strong>Tip:</strong> Count drops for 15 seconds and multiply by 4 to verify the rate
                    (should be approximately ${Math.round(dropsPerMin/4)} drops in 15 seconds)
                </div>
                <div class="formula-box" id="iv-formula" style="display:none;">
                    <strong>Formula:</strong><br>
                    Drops/min = (Volume × Drop Factor) ÷ (Time in hours × 60)<br>
                    Drops/min = (${volume} × ${dropFactor}) ÷ (${time.toFixed(2)} × 60)<br>
                    Drops/min = ${Math.round(dropsPerMin)}
                </div>
                <a class="toggle-formula" onclick="toggleFormula('iv-formula')">Show Formula</a>
            `;

            showResult('iv-result', result);
        }

        function clearIV() {
            document.getElementById('iv-volume').value = '';
            document.getElementById('iv-time').value = '';
            hideResult('iv-result');
        }

        // 3. BSA Calculator (Mosteller Formula)
        function calculateBSA() {
            let height = parseFloat(document.getElementById('bsa-height').value);
            const heightUnit = document.getElementById('bsa-height-unit').value;
            let weight = parseFloat(document.getElementById('bsa-weight').value);
            const weightUnit = document.getElementById('bsa-weight-unit').value;

            if (!height || !weight) {
                alert('Please fill in all required fields');
                return;
            }

            // Convert to metric
            if (heightUnit === 'inches') {
                height = inchesToCm(height);
            }
            if (weightUnit === 'lbs') {
                weight = lbsToKg(weight);
            }

            // Mosteller Formula: BSA (m²) = √[(Height(cm) × Weight(kg)) / 3600]
            const bsa = Math.sqrt((height * weight) / 3600);

            let category = '';
            if (bsa < 1.5) category = 'Below average';
            else if (bsa <= 2.0) category = 'Normal range';
            else category = 'Above average';

            const result = `
                <div class="result-title">Body Surface Area</div>
                <div class="result-value">${bsa.toFixed(3)} m²</div>
                <div class="result-details">
                    <strong>Height:</strong> ${height.toFixed(1)} cm<br>
                    <strong>Weight:</strong> ${weight.toFixed(1)} kg<br>
                    <strong>BSA:</strong> ${bsa.toFixed(3)} m²<br>
                    <strong>Category:</strong> ${category}
                </div>
                <div class="info-box">
                    💡 <strong>Clinical Use:</strong> BSA is commonly used for chemotherapy dosing,
                    cardiac index calculations, and medication dosing in special populations.
                </div>
                <div class="formula-box" id="bsa-formula" style="display:none;">
                    <strong>Mosteller Formula:</strong><br>
                    BSA (m²) = √[(Height(cm) × Weight(kg)) ÷ 3600]<br>
                    BSA = √[(${height.toFixed(1)} × ${weight.toFixed(1)}) ÷ 3600]<br>
                    BSA = ${bsa.toFixed(3)} m²
                </div>
                <a class="toggle-formula" onclick="toggleFormula('bsa-formula')">Show Formula</a>
            `;

            showResult('bsa-result', result);
        }

        function clearBSA() {
            document.getElementById('bsa-height').value = '';
            document.getElementById('bsa-weight').value = '';
            hideResult('bsa-result');
        }

        // 4. BMI Calculator
        function calculateBMI() {
            let height = parseFloat(document.getElementById('bmi-height').value);
            const heightUnit = document.getElementById('bmi-height-unit').value;
            let weight = parseFloat(document.getElementById('bmi-weight').value);
            const weightUnit = document.getElementById('bmi-weight-unit').value;

            if (!height || !weight) {
                alert('Please fill in all required fields');
                return;
            }

            // Convert to metric
            if (heightUnit === 'inches') {
                height = inchesToCm(height);
            }
            if (weightUnit === 'lbs') {
                weight = lbsToKg(weight);
            }

            // BMI = weight(kg) / (height(m))²
            const heightM = height / 100;
            const bmi = weight / (heightM * heightM);

            let category = '';
            let categoryColor = '';
            if (bmi < 18.5) {
                category = 'Underweight';
                categoryColor = '#2196f3';
            } else if (bmi < 25) {
                category = 'Normal weight';
                categoryColor = '#4caf50';
            } else if (bmi < 30) {
                category = 'Overweight';
                categoryColor = '#ff9800';
            } else {
                category = 'Obese';
                categoryColor = '#f44336';
            }

            const result = `
                <div class="result-title">Body Mass Index</div>
                <div class="result-value">${bmi.toFixed(1)}</div>
                <div class="result-details">
                    <strong>Height:</strong> ${height.toFixed(1)} cm (${heightM.toFixed(2)} m)<br>
                    <strong>Weight:</strong> ${weight.toFixed(1)} kg<br>
                    <strong>BMI:</strong> ${bmi.toFixed(1)} kg/m²<br>
                    <strong>Category:</strong> <span style="color: ${categoryColor}; font-weight: bold;">${category}</span>
                </div>
                <div class="info-box">
                    💡 <strong>Note:</strong> BMI is a screening tool and may not accurately reflect body composition
                    in athletes, elderly, or pregnant individuals.
                </div>
                <div class="formula-box" id="bmi-formula" style="display:none;">
                    <strong>Formula:</strong><br>
                    BMI = Weight(kg) ÷ [Height(m)]²<br>
                    BMI = ${weight.toFixed(1)} ÷ (${heightM.toFixed(2)})²<br>
                    BMI = ${bmi.toFixed(1)} kg/m²
                </div>
                <a class="toggle-formula" onclick="toggleFormula('bmi-formula')">Show Formula</a>
            `;

            showResult('bmi-result', result);
        }

        function clearBMI() {
            document.getElementById('bmi-height').value = '';
            document.getElementById('bmi-weight').value = '';
            hideResult('bmi-result');
        }

        // 5. Creatinine Clearance (Cockcroft-Gault)
        function calculateCrCl() {
            const age = parseFloat(document.getElementById('crcl-age').value);
            const weight = parseFloat(document.getElementById('crcl-weight').value);
            let scr = parseFloat(document.getElementById('crcl-scr').value);
            const scrUnit = document.getElementById('crcl-scr-unit').value;
            const sex = document.getElementById('crcl-sex').value;

            if (!age || !weight || !scr) {
                alert('Please fill in all required fields');
                return;
            }

            // Convert SCr to mg/dL if in µmol/L
            if (scrUnit === 'µmol/L') {
                scr = umolToMg(scr);
            }

            // Cockcroft-Gault: CrCl = [(140 - age) × weight] / (72 × SCr) × (0.85 if female)
            let crcl = ((140 - age) * weight) / (72 * scr);
            if (sex === 'female') {
                crcl = crcl * 0.85;
            }

            let category = '';
            let categoryColor = '';
            if (crcl >= 90) {
                category = 'Normal';
                categoryColor = '#4caf50';
            } else if (crcl >= 60) {
                category = 'Mild impairment';
                categoryColor = '#8bc34a';
            } else if (crcl >= 30) {
                category = 'Moderate impairment';
                categoryColor = '#ff9800';
            } else if (crcl >= 15) {
                category = 'Severe impairment';
                categoryColor = '#f44336';
            } else {
                category = 'Kidney failure';
                categoryColor = '#d32f2f';
            }

            const result = `
                <div class="result-title">Creatinine Clearance</div>
                <div class="result-value">${crcl.toFixed(1)} mL/min</div>
                <div class="result-details">
                    <strong>Age:</strong> ${age} years<br>
                    <strong>Weight:</strong> ${weight} kg<br>
                    <strong>Serum Creatinine:</strong> ${scr.toFixed(2)} mg/dL<br>
                    <strong>Sex:</strong> ${sex.charAt(0).toUpperCase() + sex.slice(1)}<br>
                    <strong>CrCl:</strong> ${crcl.toFixed(1)} mL/min<br>
                    <strong>Renal Function:</strong> <span style="color: ${categoryColor}; font-weight: bold;">${category}</span>
                </div>
                <div class="warning-box">
                    <div class="warning-title">⚠️ Important</div>
                    Cockcroft-Gault may overestimate CrCl in obese patients and elderly.
                    Consider using actual body weight for obese patients or ideal body weight for better accuracy.
                </div>
                <div class="formula-box" id="crcl-formula" style="display:none;">
                    <strong>Cockcroft-Gault Formula:</strong><br>
                    CrCl = [(140 - Age) × Weight] ÷ (72 × SCr) ${sex === 'female' ? '× 0.85' : ''}<br>
                    CrCl = [(140 - ${age}) × ${weight}] ÷ (72 × ${scr.toFixed(2)}) ${sex === 'female' ? '× 0.85' : ''}<br>
                    CrCl = ${crcl.toFixed(1)} mL/min
                </div>
                <a class="toggle-formula" onclick="toggleFormula('crcl-formula')">Show Formula</a>
            `;

            showResult('crcl-result', result);
        }

        function clearCrCl() {
            document.getElementById('crcl-age').value = '';
            document.getElementById('crcl-weight').value = '';
            document.getElementById('crcl-scr').value = '';
            hideResult('crcl-result');
        }

        // 6. Dilution Calculator
        function calculateDilution() {
            const stockConc = parseFloat(document.getElementById('dil-stock-conc').value);
            const desiredConc = parseFloat(document.getElementById('dil-desired-conc').value);
            const finalVolume = parseFloat(document.getElementById('dil-final-volume').value);
            const unit = document.getElementById('dil-stock-unit').value;

            if (!stockConc || !desiredConc || !finalVolume) {
                alert('Please fill in all required fields');
                return;
            }

            if (desiredConc >= stockConc) {
                alert('Desired concentration must be less than stock concentration');
                return;
            }

            // C1V1 = C2V2
            // V1 = (C2 × V2) / C1
            const stockVolume = (desiredConc * finalVolume) / stockConc;
            const diluent = finalVolume - stockVolume;

            const result = `
                <div class="result-title">Dilution Instructions</div>
                <div class="result-value">Use ${stockVolume.toFixed(2)} mL of stock</div>
                <div class="result-details">
                    <strong>Stock Concentration:</strong> ${stockConc} ${unit}<br>
                    <strong>Desired Concentration:</strong> ${desiredConc} ${unit}<br>
                    <strong>Final Volume:</strong> ${finalVolume} mL<br>
                    <br>
                    <strong>Stock Volume Needed:</strong> ${stockVolume.toFixed(2)} mL<br>
                    <strong>Diluent to Add:</strong> ${diluent.toFixed(2)} mL<br>
                    <strong>Total Volume:</strong> ${finalVolume} mL
                </div>
                <div class="info-box">
                    📋 <strong>Instructions:</strong><br>
                    1. Measure ${stockVolume.toFixed(2)} mL of stock solution<br>
                    2. Add ${diluent.toFixed(2)} mL of appropriate diluent (water, saline, etc.)<br>
                    3. Mix thoroughly<br>
                    4. Final concentration will be ${desiredConc} ${unit}
                </div>
                <div class="formula-box" id="dil-formula" style="display:none;">
                    <strong>C₁V₁ = C₂V₂ Formula:</strong><br>
                    V₁ = (C₂ × V₂) ÷ C₁<br>
                    V₁ = (${desiredConc} × ${finalVolume}) ÷ ${stockConc}<br>
                    V₁ = ${stockVolume.toFixed(2)} mL
                </div>
                <a class="toggle-formula" onclick="toggleFormula('dil-formula')">Show Formula</a>
            `;

            showResult('dil-result', result);
        }

        function clearDilution() {
            document.getElementById('dil-stock-conc').value = '';
            document.getElementById('dil-desired-conc').value = '';
            document.getElementById('dil-final-volume').value = '';
            hideResult('dil-result');
        }

        // Toggle formula display
        function toggleFormula(id) {
            const formula = document.getElementById(id);
            if (formula.style.display === 'none') {
                formula.style.display = 'block';
            } else {
                formula.style.display = 'none';
            }
        }
    </script>
</body>
</html>
