<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Калькулятор углеродного следа для жителя Санкт-Петербурга</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background-color: #f0f8f0;
        }

        .calculator {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .input-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            color: #2c5f2d;
        }

        input, select {
            width: 100%;
            padding: 8px;
            border: 1px solid #97bc62;
            border-radius: 4px;
            box-sizing: border-box;
        }

        button {
            background-color: #2c5f2d;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            font-size: 16px;
        }

        button:hover {
            background-color: #1e401f;
        }

        #result {
            margin-top: 20px;
            font-size: 18px;
            text-align: center;
            padding: 15px;
            background-color: #e9f5e9;
            border-radius: 5px;
        }

        .sources {
            margin-top: 30px;
            font-size: 12px;
            color: #666;
            text-align: center;
        }

        .disclaimer {
            margin-top: 20px;
            font-size: 12px;
            color: #888;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h2>Калькулятор углеродного следа для жителя Санкт-Петербурга</h2>
        
        <div class="input-group">
            <label>Электричество (кВт·ч в месяц):</label>
            <input type="number" id="electricity" min="0">
        </div>

        <div class="input-group">
            <label>Тип автомобиля:</label>
            <select id="carType">
                <option value="petrol">Бензин</option>
                <option value="diesel">Дизель</option>
                <option value="electric">Электрокар</option>
            </select>
        </div>

        <div class="input-group">
            <label>Пробег на автомобиле (км в месяц):</label>
            <input type="number" id="carMileage" min="0">
        </div>

        <div class="input-group">
            <label>Тип общественного транспорта:</label>
            <select id="publicTransportType">
                <option value="bus">Автобус</option>
                <option value="tram">Трамвай</option>
                <option value="metro">Метро</option>
                <option value="trolleybus">Троллейбус</option>
            </select>
        </div>

        <div class="input-group">
            <label>Поездки на общественном транспорте (км в месяц):</label>
            <input type="number" id="publicTransport" min="0">
        </div>

        <div class="input-group">
            <label>Потребление газа (м³ в месяц):</label>
            <input type="number" id="gas" min="0">
        </div>

        <button onclick="calculateFootprint()">Рассчитать</button>

        <div id="result"></div>
    </div>

    <div class="sources">
        <h3>Источники данных:</h3>
        <ul>
            <li>Данные о коэффициентах брались: <a href="https://www.epa.gov" target="_blank">EPA (2022)</a></li>, <a href="https://www.iea.org" target="_blank">IEA (2022)</a>, <a href="https://www.ipcc-nggip.iges.or.jp" target="_blank">IPCC (2022)</a>, <a href="https://www.carbonfootprint.com" target="_blank">Carbon Footprint (2022)</a></li>
        </ul>
    </div>

    <div class="disclaimer">
        <p>
            <strong>Примечание:</strong> Все данные, используемые в этом калькуляторе, являются средними и не могут быть максимально точными из-за различий в условиях эксплуатации, технологиях и региональных особенностях. Результаты носят ознакомительный характер.
        </p>
    </div>

    <script>
        function calculateFootprint() {
            // Коэффициенты выбросов (кг CO2)
            const coefficients = {
                electricity: 0.45,    // на 1 кВт·ч (СПб, смешанная генерация)
                petrol: 0.18,         // на 1 км (бензин)
                diesel: 0.16,         // на 1 км (дизель)
                electricCar: 0.05,   // на 1 км (электрокар, с учетом выбросов при генерации)
                bus: 0.1,             // на 1 км (автобус)
                tram: 0.05,           // на 1 км (трамвай)
                metro: 0.03,          // на 1 км (метро)
                trolleybus: 0.04,     // на 1 км (троллейбус)
                gas: 1.89            // на 1 м³ (природный газ)
            };

            // Получение значений
            const electricity = parseFloat(document.getElementById('electricity').value) || 0;
            const carType = document.getElementById('carType').value;
            const carMileage = parseFloat(document.getElementById('carMileage').value) || 0;
            const publicTransportType = document.getElementById('publicTransportType').value;
            const publicTransport = parseFloat(document.getElementById('publicTransport').value) || 0;
            const gas = parseFloat(document.getElementById('gas').value) || 0;

            // Расчет выбросов для автомобиля
            let carEmissions = 0;
            switch (carType) {
                case 'petrol':
                    carEmissions = carMileage * coefficients.petrol;
                    break;
                case 'diesel':
                    carEmissions = carMileage * coefficients.diesel;
                    break;
                case 'electric':
                    carEmissions = carMileage * coefficients.electricCar;
                    break;
            }

            // Расчет выбросов для общественного транспорта
            let publicTransportEmissions = 0;
            switch (publicTransportType) {
                case 'bus':
                    publicTransportEmissions = publicTransport * coefficients.bus;
                    break;
                case 'tram':
                    publicTransportEmissions = publicTransport * coefficients.tram;
                    break;
                case 'metro':
                    publicTransportEmissions = publicTransport * coefficients.metro;
                    break;
                case 'trolleybus':
                    publicTransportEmissions = publicTransport * coefficients.trolleybus;
                    break;
            }

            // Общий расчет
            const total = 
                (electricity * coefficients.electricity) +
                carEmissions +
                publicTransportEmissions +
                (gas * coefficients.gas);

            // Годовая оценка
            const annualTotal = total * 12;

            // Отображение результата
            const resultDiv = document.getElementById('result');
            resultDiv.innerHTML = `
                <strong>Ежемесячный углеродный след:</strong> ${total.toFixed(2)} кг CO2<br>
                <strong>Годовой углеродный след:</strong> ${annualTotal.toFixed(2)} кг CO2<br><br>
                <small>(Для сравнения: средний россиянин производит около 12,000 кг CO2 в год)</small>
            `;
        }
    </script>
</body>
</html>
