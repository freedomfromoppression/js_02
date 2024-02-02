# js_02
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>움직이는 볼</title>
    <style>
        canvas{background-color: yellow; border: 1px dotted black;
        width: 300px; height: 200px;}
    </style>
</head>
<body>
    <h3>움직이는 볼</h3>
    <hr>
    <canvas id="myCanvas"></canvas>
    <script>
        let canvas = document.getElementById("myCanvas");
        let context = canvas.getContext("2d");
        let dx =5;
        let dy = 5;
        let x = 100;
        let y = 100;
        function fn_draw(){
            context.clearRect(0, 0, canvas.width, canvas.height);
            context.beginPath();
            context.fillStyle='red';
            //원그리는 함수
            context.arc(x,y,20,0, Math.PI * 2, true);
            context.closePath();
            context.fill();
            if( x<20 || x>280){
                dx = -dy;
            }
            if( y<20 || y>180){
                dy= -dy;
            }
            x+=dx;
            y+=dy;
            requestAnimationFrame(fn_draw);
            console.log("움직임");
        }
        setInterval(fn_draw, 20);
    </script>
    
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>텍스트 출력</title>
</head>
<body>
    <canvas id ="myCanvas" width="500px" height="100px" style="background-color: beige;"></canvas><br>
    <input type="text" id="input_id" size="30" placeholder="입력하세요"
    onkeyup="fn_text(this.value)">
    <script>
        let canvas = document.getElementById("myCanvas");
        let context = canvas.getContext("2d");
        context.font = "50px fore";
        context.strokeStyle = 'magenta';
        context.lineWidth = 1;
        context.textAlign = 'left';
        function fn_text(text){
            //사이즈 만큼 지우기
            context.clearRect(0, 0, canvas.width, canvas.height);
            context.strokeText(text, 40, 80);
        }
    </script>
    
</body>
</html>
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>그림판</title>
    <style>
        canvas{cursor:url(cursor\ \(1\).cur),auto}
    </style>
</head>

<body onload="init()">
    <h3>마우스를 누른채 드래깅하여 그림을 그려요</h3>
    <hr>
    <canvas id="myCanvas" width="400px" height="300px" style="background-color: aliceblue;"></canvas>
    <div>
        <table>
            <tr>
                <td>컬러</td><td><input type="color" id="p_color" onchange="fn_change(this)"></td>
            </tr>
            <tr>
                <td>사이즈</td><td><input type="range" min="1" max="30" value="1"></td>
            </tr>
            <tr>
                <td>
                    그림 <input type = "radio" name="tool" value="p" checked onchange="fn_change(this)">
                    지우개 <input type="radio" name="tool" value="E" onchange="fn_change(this)">
                </td>
                <td><button type= "button" onclick="fn_clear()">전체삭제</button><button onclick="fn_save()"></button></td>
            </tr>
        </table>
    </div>
    <script>
        let canvas, context;
        let startX = 0, startY = 0;
        let dragging = false;
        let tools = 'B'; //B 그림, E 지우개 
        function init() {
            canvas = document.getElementById("myCanvas");
            context = canvas.getContext("2d");
            context.lineCap = 'round';
            context.strokeStyle = 'black';
            context.lineWidth = 2;
            canvas.addEventListener('mousedown', down);
            canvas.addEventListener('mouseup', up);
            canvas.addEventListener('mousemove', move);
            canvas.addEventListener('mouseout', out);
        }
        function down(e) {
            startX = e.offsetX;
            startY = e.offsetY;// 클릭하여 움직임을 시작하는 위치
            dragging = true;
        }
        function up(e) {
            diagging = false; //움직여도 더이상 그려지지않도록
        }
        function out(e) {
            diagging = false;
        }
        function move(e) {
            if (!dragging) {
                return;
            }
            let curX = e.offsetX;
            let curY = e.offsetY;
            draw(curX, curY);
            startX = curX;
            startY = curY;
            }

        function draw(curX, curY) {
            if(tppls =='E'){
                context.clearRect(curX, curY, 5, 30)
            }else{
                context.beginPath();
                context.moveTo(startX, startY);
                context.lineTo(curX, curY);
                context.stroke();
                }
        }
        function fn_clear(){
            context.clearRect(0, 0, canvas.width, canvas.height);
        }
        function fn_change(obj){
            if(obj.id== 'p_color'){
                context.strokeStyle = obj.value;
            }else if(obj.id == 'p_size'){
                context.lineWidth = obj.value;
            }else if(obj.name == 'tool'){
                tool = obj.valus;
                if(tools == 'E'){
                    canvas.style.cursor = 'X url(e.cur), auto';
                }else{
                    canvas.style.cursor = 'Y url(b.cur), auto';
                }
            }
        }
        function fn_save(){
            //임시 캔버스 생성
            let tempCanvas = document.createElement("canvas");
            tempCanvas.width = canvas.width;
            tempCanvas.height = canvas.height;
            //임시 캔버스에 배경색 채우기
            let tempContext = tempCanvas.getContext("2d");
            tempContext.fillStyle = 'aliceblue';
            tempContext.fillRect(0,0,tempCanvas.width, tempCanvas.height);
            //원본 복사내용
            tempContext.drawImage(canvas,0,0);

            let img = tempCanvas.toDataURL("image/png").replace('image/png','image/octet-stream');
            let temp = document.createElement('a');
            temp.download = 'my-canvas.png';
            temp.href = img;
            temp.click();
        }
    </script>

</body>

</html>
