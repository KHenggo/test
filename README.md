<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>금액 계산기</title>
</head>
<body>
    <h1>금액 계산기</h1>

    <!-- 인출 금액과 테더 환율 입력 -->
    <label for="withdraw-amount">인출 금액 (USD): </label>
    <input type="number" id="withdraw-amount" placeholder="USD 금액 입력" step="0.01"><br><br>

    <label for="tether-rate">테더 환율 (KRW/USD): </label>
    <input type="number" id="tether-rate" placeholder="환율 입력" step="0.01"><br><br>

    <button onclick="calculatePayout()">계산하기</button>

    <h2>계산 과정</h2>
    <div id="calculation-process"></div>

    <script>
        function calculatePayout() {
            const withdrawAmount = parseFloat(document.getElementById('withdraw-amount').value);
            const tetherRate = parseFloat(document.getElementById('tether-rate').value);

            let resultText = '';

            // 단계 1: OAM 인출 금액
            const oamAmount = withdrawAmount;
            resultText += `1. OAM 인출 금액: ${oamAmount.toFixed(2)} USD<br>`;

            // 단계 2: OMA -> 바이넌스 이동 (3% 수수료)
            const binanceAmount = withdrawAmount * 0.97;
            resultText += `2. OMA -> 바이넌스 이동 (3% 수수료): ${withdrawAmount.toFixed(2)} × 0.97 = ${binanceAmount.toFixed(2)} USD<br>`;

            // 단계 3: 바이넌스 -> 빗썸 이동 (1 USD 수수료)
            const bithumbAmount = binanceAmount - 1;
            resultText += `3. 바이넌스 -> 빗썸 이동 (1 USD 수수료): ${binanceAmount.toFixed(2)} - 1 = ${bithumbAmount.toFixed(2)} USD<br>`;

            // 단계 4: 당일 테더금액
            const tetherAmount = bithumbAmount * tetherRate;
            resultText += `4. 당일 테더금액 (환율 ${tetherRate} KRW): ${bithumbAmount.toFixed(2)} × ${tetherRate.toFixed(2)} = ${tetherAmount.toFixed(2)} KRW<br>`;

            // 단계 5: 빗썸 -> 통장 이동
            const finalAmount = tetherAmount - 1000;
            resultText += `5. 빗썸 -> 통장 이동 (1,000 KRW 수수료): ${tetherAmount.toFixed(2)} - 1,000 = ${finalAmount.toFixed(2)} KRW<br>`;

            // 단계 6: 최종 수령 금액 (1% 수수료)
            const finalResult = finalAmount * 0.99;
            resultText += `6. 최종 수령 금액 (1% 수수료): ${finalAmount.toFixed(2)} × 0.99 = ${finalResult.toFixed(2)} KRW`;

            // 결과 출력
            document.getElementById('calculation-process').innerHTML = resultText;
        }
    </script>
</body>
</html>
