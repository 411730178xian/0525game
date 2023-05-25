# 0525game
# 大象射擊最新0525

![](https://hackmd.io/_uploads/B1y9U9nHn.png)


## sketch.js
```javascript=
// let points = [
// [7,10],[12,6],[12,4],[9,1],[10,-2],[10,-7],[5,-10],[1,-11],[1,-13],[-3,-13],[-14,-4],[-13,4],
// [-11,9],[-12,13],[-10,16],[-8,17],[-5,13],[3,13],[7,16],[10,15],[10,13],[7,10]
// ]


// let points =[[6, -3], [5, 0], [7, 2],[7,4],[6,5],[9,5],[9,6],[8,7],[7,8],[6,8],[5,10],[4,10],[4,9],[5,8],[4,5],[0,5],[-2,4],[-4,1],[-4,-6],[-5,-7],[-10,-6],[-9,-7],[-4,-8],[-3,-7],[-1,-5],[4,4],[3,2],[3,1],[5,-3],[4,-4],[5,-4],[6,-3],[4,1],[5,2],[1,-4],[2,-5],[2,-8],[8,-8],[7,-7],[3,-7],[3,-1],[4,-1],[3,-1],[2,-3],[0,-5],[-4,-2],[-3,-4],[-1,-5],[-1,-9],[5,-10],[6,-9],[0,-8],[0,-5],[1,0],[-1,3],[5,-4],[6,-4],[7,-3],[6,1]];

let points = [[-2, 0], [-1,-1], [0, -1],[1,0],[1,2],[0,3],[-1,3],[-2,2],[-3,2],[-4,1],[-4,-2],[-5,-4],[-4,-4],[-3,-2],[-2,-1],[-2,-3], [-2,-4], [-1, -4],[0,-4],[0,-2],[2,-2],[2,-4], [4, -4],[4,1],[3,2],[1,2],[1,2]]; //list資料，
var fill_colors = "fec5bb-fcd5ce-fae1dd-f8edeb-e8e8e4-d8e2dc-ece4db-ffe5d9-ffd7ba-fec89a".split("-").map(a=>"#"+a)
var line_colors = "cdb4db-ffc8dd-ffafcc-bde0fe-a2d2ff".split("-").map(a=>"#"+a)
//class::類別，例子

//+++++++++++++++畫points所有點的物件定義
var ball //目前要處理的物件，暫時放在ball變數內
var balls =[] //把產生的"所有"的物件，為物件的倉庫，所有的物件資料都在此

//+++++++++++設定飛彈物件的變數
var bullet //"目前要處理"的物件，暫時放在bullet變數內
var bullets = [] //把產生"所有的物件，為物件的倉庫，所有的物件皆在此

//+++++++++++設定怪物物件的變數
var monster //"目前要處理"的物件，暫時放在bullet變數內
var monsters = [] //把產生"所有的物件，為物件的倉庫，所有的物件皆在此
//+++++++++++++++++++++++++++++++

//+++++++++設定砲台位置
var shipP

//++++++++++++++++++++++++++++++++
var score=0

function preload(){ //程式碼準備執行之前，所執行的程式碼內容，比setup更早執行
  elephant_sound = loadSound("sound/elephant.wav")
  bullet_sound = loadSound("sound/Launching wire.wav")
}


function setup() {
  createCanvas(windowWidth, windowHeight);
  shipP=createVector(width/2,height/2) //預設砲台位置為(width/2,height/2)
    //產生大象
    for(var i=0;i<25;i=i+1){
      ball = new obj({}) //產生一個obj class元件
      balls.push(ball) //把ball的物件放到balls陣列內
    }

    //產生怪物
    for(var i=0;i<20;i=i+1){
      monster = new Monster({}) //產生一個obj class元件
      monsters.push(monster) //把monster的物件放到monsters陣列內
    }
}



function draw() {
  background(220);
  // for(var j=0;j<balls.length;j++){
  //   ball = balls[j]
  //   ball.draw()
  //   ball.update()
  // }

  if(keyIsPressed){
    if(key=="ArrowLeft" || key=="a"){ //按下鍵盤左鍵
      shipP.x=shipP.x-5
    }
    if(key=="ArrowRight" || key=="d"){ //按下鍵盤右鍵
      shipP.x=shipP.x+5
    }
    if(key=="ArrowUp" || key=="w"){ //按下鍵盤上鍵
      shipP.y=shipP.y-5
    }
    if(key=="ArrowDown" || key=="s"){ //按下鍵盤下鍵
      shipP.y=shipP.y+5
    }
  }
  
  //大象的顯示
  for(let ball of balls) //只要是陣列的方式，都可以用此方式處理
  {
    ball.draw()
    ball.update()
      for(let bullet of bullets){ //檢查每一個物件
        if(ball.isBallinranger(bullet.p.x,bullet.p.y)){ //inranger，判斷有無碰到
        balls.splice(balls.indexOf(ball),1) //從倉庫balls取出被滑鼠按到的物件編號(balls.lastIndexOf(ball))，只取1個
        bullets.splice(bullets.indexOf(bullet),1)
        score=score - 1
        elephant_sound.play()
       }
    }
  }

//飛彈的顯示
for(let bullet of bullets) //只要是陣列的方式，都可以用此方式處理
  {
    bullet.draw()
    bullet.update()
  }

  //怪物的顯示
  for(let monster of monsters) //只要是陣列的方式，都可以用此方式處理
  {
    if(monster.dead == true && monster.timenumber>4){
    monsters.splice(monsters.indexOf(monster),1)
    }
    monster.draw()
    monster.update()
    for(let bullet of bullets){ //檢查每一個物件
      if(monster.isBallinranger(bullet.p.x,bullet.p.y)){ //inranger，判斷有無碰到
      // monsters.splice(monsters.indexOf(monster),1) //從倉庫monsters取出被滑鼠按到的物件編號(balls.lastIndexOf(ball))，只取1個
      bullets.splice(bullets.indexOf(bullet),1)
      score=score + 1
      monster.dead = true //代表怪物死亡
      // elephant_sound.play()
     }
  }
  }


textSize(50)
text(score,50,50) //在座標為(50,50)上顯示score分數內容
  push() //重新規劃原點(0,0)，在視窗的中間
  let dx=mouseX-width/2
  let dy=mouseY-height/2
  let angle=atan2(dy,dx) //dy分子，dx分母
  // translate(width/2,height/2) //把砲台中心點放在視窗中間
  translate(shipP.x,shipP.y)
  fill("#f48c06")
  noStroke()
  rotate(angle)
  //  triangle(-25,25,25,25,0,-50) //設定三個眼，畫成一個三角形
  triangle(50,0,-25,25,-25,-25)
  fill("#118ab2")
  ellipse(0,0,25)
  pop()  //恢復原本設定，原點(0,0)在視窗左上角

}


function mousePressed(){
  //++++++++++++++++產生一個物件+++++++++++++++
  // ball = new obj({  //產生一個obj class元件
  //   p:{x:mouseX,y:mouseY}
  // }) //在滑鼠按下的地方產生一個新的obj class元件
  // balls.push(ball) //把ball的物件放到balls陣列內
  //++++++++++++++++++++++++++++++++++++++++++++

  //+++++在物件上按下滑鼠，物件消失不見，分數加1分++++++++++++++++

  // for(let ball of balls){ //檢查每一個物件
  //   if(ball.isBallinranger(mouseX,mouseY)){
  //     balls.splice(balls.indexOf(ball),1) //從倉庫balls取出被滑鼠按到的物件編號(balls.lastIndexOf(ball))，只取1個
  //     score=score + 1
  //   }
  // }
  //++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

  //+++++++++++++++++++++按一下產生一個飛彈+++++++++++++++++++++++
  bullet = new Bullet({
    r:20 //可以自己加參數color,v...
  }) //在滑鼠按下的地方，產生一個新的bullet class元件
  bullets.push(bullet) //把bullet的物件放入到bullet陣列內(丟到倉庫)
  bullet_sound.play()
}

function keyPressed(){
  if(key==" "){ //按下空白建，發射飛彈，跟按下滑鼠功能一樣
    bullet = new Bullet({
      r:20 //可以自己加參數color,v...
    }) //在滑鼠按下的地方，產生一個新的bullet class元件
    bullets.push(bullet) //把bullet的物件放入到bullet陣列內(丟到倉庫)
    bullet_sound.play()
  }
}
```

## bullet.js
```javascript=
//定義一個bullet物件的class
class Bullet{
    constructor(args){ //預設值，基本資料(物件的顏色，移動的速度，大小，初始顯示位置...)
      this.r = args.r || 10 //設計的飛彈有大有小時，就傳參數args.r來設定飛彈大小
      // this.p = args.p || createVector(width/2,height/2) //建立一個向量，{x:width/2,y:height/2} 
      this.p = args.p || shipP.copy()
      this.v = args.v ||createVector(mouseX-width/2,mouseY-height/2).limit(10) //算出方向，limit-->每次移動5
      this.color = args.color || "#82c0cc"
    }
    draw(){ //繪出物件程式碼
        push() //
         translate(this.p.x,this.p.y) //以該物件位置為原點
         fill(this.color)
         noStroke()
         ellipse(0,0,this.r)
        pop()
    }
    update(){ //計算出移動後的位置
        // this.p.x = this.p.x + this.v.x //X軸目前位置(this.p.x)加X軸的移動速度(this.v.x )
        // this.p.y = this.p.y + this.v.y
        this.p.add(this.v)
  
    }
  }
```

## monster.js
```javascript=
var colors1 = "8a817c-70798c-f5f1ed-dad2bc-a99985".split("-").map(a=>"#"+a)

class Monster{  //宣告一個怪物類別，名稱為Monster
    constructor(args){ //預設值，基本資料(物件的顏色，移動的速度，大小，初始顯示位置...)
        this.r = args.r || random(50,130) //設計怪物的主體，如果傳參數args.r來設定怪物大小，沒有就100
        this.p = args.p || createVector(random(width),random(height)) //建立一個向量，由電腦亂數抽取顯示的初始位置，{x:random(width),y:random(height)} 
        this.v = args.v ||createVector(random(-1,1),random(-1,1)).limit(10) //移動的速度，如果沒有傳args參數，就會用亂數(-1,1)，抽出x,y軸移動速度
        this.color =args.color || random(colors1)//充滿顏色
        this.mode = random(["happy","bad"])
        this.dead = false //false代表活著
        this.timenumber = 0 //延長時間，讓大家看到他死
      }
    draw(){ //劃出元件
        if (this.dead==false){
            push() //重新設定原點位置
            translate(this.p.x,this.p.y)//把原點(0,0)座標移到物件中心位置
            fill(this.color)
            noStroke()
            ellipse(0,0,this.r)
            if(this.mode=="happy"){
                fill(255)
                ellipse(0,0,this.r/2)
                fill(0)
                ellipse(0,0,this.r/3)
            }
            else{
                fill(255)
                arc(0,0,this.r/2,this.r/2,0,PI) //arc弧度，0~180度
                fill(0)
                arc(0,0,this.r/3,this.r/3,0,PI) //arc弧度，0~180度
            }

            //+++++++++++++++++++++++++++++++++++++++++
            stroke(this.color)
            strokeWeight(4)
            // line(this.r/2,0,this.r,0)
            noFill()
            for (var j=0;j<8;j++){
                rotate(PI/4) //翻轉
                beginShape()
                for(var i=0;i<(this.r/2);i++){
                    vertex(this.r/2+i,sin(i/5+frameCount/10)*10)
                }
                endShape()
            }
            
            
            pop() //恢復原點到視窗左上角
        }
        else{ //怪物死亡
                this.timenumber = this.timenumber+1
            push()
                translate(this.p.x,this.p.y)//把原點(0,0)座標移到物件中心位置
                fill(this.color)
                noStroke()
                ellipse(0,0,this.r)
                stroke(255)
                line(-this.r/2,0,this.r/2,0)
                stroke(this.color)
                strokeWeight(4)
                noFill()
                //line(-this.r/2,0,this.r/2,0)
                for(var j=0;j<4;j++){
                    rotate(PI/2)
                    line(this.r,0,this.r/2,0)
                }
            pop()
        }
    }
    update(){ ////計算出元件移動後的位置
      this.p.add(this.v)

     //碰到牆壁會回彈
        if(this.p.x<=0 || this.p.x>=width){ //X軸碰到左邊(<=0)，或是碰到右邊(>=width)
            this.v.x=-this.v.x //把速度方向改變
        }
        if(this.p.y<=0 || this.p.y>=height-50){ //X軸碰到上邊(<=0)，或是碰到下邊(>=height)
        this.v.y=-this.v.y //把速度方向改變
        }
    }
    isBallinranger(x,y){ //功能:判斷滑鼠按下的位置是否在物件的範圍內
        let d = dist(x,y,this.p.x,this.p.y) //計算兩點(滑鼠按下與物件中心點)之間的距離，放到d變數內
        if(d<this.r/2){
          return true //滑鼠與物件的距離小於物件的寬度，代表碰觸了，則傳回true的值(碰觸)
        }else{
          return false //滑鼠與物件的距離大於物件的寬度，代表未碰觸，則傳回false的值(未碰觸)
        }
    }
}
```

## obj.js
```javascript=
class obj{ //宣告一個類別，針對一個畫的圖案
    constructor(args){ //預設值，基本資料(物件的顏色，移動的速度，大小，初始顯示位置...)
    // this.p = args.p || {x:random(width),y:random(height)} //描述為該物件的初始位置，||(or)，當產生一個物件時，有傳給位置參數，使用該參數，如果沒有傳參數，則使用or後面的產出
    this.p = args.p || createVector(random(width),random(height)) 
    // this.v = {x:random(-1,1),y:random(-1,1)} //設定一個物件的移動速度
    this.v = createVector(random(-1,1),random(-1,1)) //把原本的{X:..Y;..}改成"向量"方式呈現
    this.size =random(10,30) //一個物件的放大倍率
    this.color =random(fill_colors) //充滿顏色
    this.stroke =random(line_colors) //外框線條顏色
  }

  draw(){ //劃出單一個物件形狀
    push() //依照我的設定，設定原點(0,0)的位置
     translate(this.p.x,this.p.y) //以該物件位置為原點
     scale(this.v.x<0?1:-1,-1) //前面條件成立?否則:。如果this.v.x<0條件成立，值唯一，否則為-1，y軸的-1，為上下翻轉。
     fill(this.color)
     stroke(this.stroke)
     strokeWeight(4) //線條粗細
     beginShape()
     for(var k=0;k<points.length;k++){
      // line(points[k][0]*this.size,points[k][1]*this.size,points[k+1][0]*this.size,points[k+1][1]*this.size) //需要提供兩個點的座標
      // vertex(points[k][0]*this.size,points[k][1]*this.size) //只要設定一個點，當指令到endShape()，會把所有點串接在一起
      curveVertex(points[k][0]*this.size,points[k][1]*this.size) //用圓弧畫
     }
     endShape()
    pop() //執行pop()，原點(0，0)設定回到整個視窗的左上角
  }
  update(){
    // this.p.x = this.p.x + this.v.x //X軸目前位置(this.p.x)加X軸的移動速度(this.v.x )
    // this.p.y = this.p.y + this.v.y
    this.p.add(this.v) //設定好向量後，使用add就可以與上面兩行指令一樣的效果
    //向量sub==>減號
    //知道滑鼠的位置，並建立一個滑鼠的向量
    // let mouseV=createVector(mouseX,mouseY) //把滑鼠的位置轉換成一個向量值
    // let delta =mouseV.sub(this.p).limit(this.v.mag()*2) //sub計算出滑鼠所在位置向量(mouseV)到物件向量(this.v)的距離，每次以3的距離，this.v.mag()代表該物件的速度大小(一個向量值有大小與方向)
    // this.p.add(delta)
    
    //碰到牆壁會回彈
    if(this.p.x<=0 || this.p.x>=width){ //X軸碰到左邊(<=0)，或是碰到右邊(>=width)
      this.v.x=-this.v.x //把速度方向改變
    }
  if(this.p.y<=0 || this.p.y>=height-50){ //X軸碰到上邊(<=0)，或是碰到下邊(>=height)
    this.v.y=-this.v.y //把速度方向改變
  }
  }
  isBallinranger(x,y){ //功能:判斷滑鼠按下的位置是否在物件的範圍內
    let d = dist(x,y,this.p.x,this.p.y) //計算兩點(滑鼠按下與物件中心點)之間的距離，放到d變數內
    if(d<4*this.size){
      return true //滑鼠與物件的距離小於物件的寬度，代表碰觸了，則傳回true的值(碰觸)
    }else{
      return false //滑鼠與物件的距離大於物件的寬度，代表未碰觸，則傳回false的值(未碰觸)
    }

  }
}
```

## index.html
```javascript=
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Sketch</title>

    <link rel="stylesheet" type="text/css" href="style.css">

    <script src="libraries/p5.min.js"></script>
    <script src="libraries/p5.sound.min.js"></script>
  </head>

  <body>
    <script src="sketch.js"></script>
    <script src="obj.js"></script>
    <script src="bullet.js"></script>
    <script src="monster.js"></script>
  </body>
</html>

```
