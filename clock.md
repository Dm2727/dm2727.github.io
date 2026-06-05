<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CLOCKEEY</title>
<style>
*{box-sizing:border-box}
body,button,textarea{font-family:"Arial Rounded MT Bold","Arial Rounded MT",Arial,sans-serif}
body{margin:0;padding:20px;background:#0f2e18;color:#e6ffe6}
.topRow{display:flex;gap:40px;align-items:center;justify-content:center;flex-wrap:wrap}
#digital{font-size:42px;font-weight:bold;color:#b7ffb7}
#calendarText{font-size:24px}
.clock{width:500px;height:500px;border:10px solid #1f3a2a;border-radius:50%;position:relative;background:#102418}
.number{position:absolute;width:50px;height:50px;display:flex;align-items:center;justify-content:center;transform:translate(-50%,-50%);font-size:32px;font-weight:bold}
.hand{position:absolute;top:50%;left:50%;transform-origin:0% 50%}
#hour{width:30%;height:8px;background:#8cff8c}
#minute{width:40%;height:6px;background:#d6ffd6}
#second{width:45%;height:3px;background:#fff}
#ms{width:48%;height:2px;background:#00ff66}
.center-dot{width:24px;height:24px;border-radius:50%;background:white;position:absolute;top:238px;left:238px}
#yearCalendar{margin-top:20px;height:900px;overflow:auto;background:#163520;padding:15px;border-radius:10px}
.year{margin-bottom:20px}
.months{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:10px}
.month{background:#102418;padding:8px;border-radius:8px}
.month h4{margin:0 0 8px;color:#7dff7d}
.days{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;font-size:12px}
.day,.hdr{text-align:center;padding:3px}
.hdr{font-weight:bold}
.day{border:1px solid #2e6b42;cursor:pointer}
.hasNote{background:#006b3a}
textarea{width:100%;height:120px}
#notePanel{display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#102418;padding:15px;border:1px solid #2e6b42}
</style>
</head>
<body>
<div class="topRow">
<div>
<h1>CLOCKEEY</h1>
<div id="digital"></div>
<div id="calendarText"></div>
</div>
<div class="clock">
<div id="nums"></div>
<div id="hour" class="hand"></div>
<div id="minute" class="hand"></div>
<div id="second" class="hand"></div>
<div id="ms" class="hand"></div>
<div class="center-dot"></div>
</div>
</div>
<div id="yearCalendar"></div>

<div id="notePanel">
<h3 id="noteDate"></h3>
<textarea id="noteText"></textarea><br>
<button onclick="saveNote()">Save</button>
<button onclick="closeNote()">Close</button>
</div>

<script>
const months=["January","February","March","April","May","June","July","August","September","October","November","December"];
const days=["Mon","Tue","Wed","Thu","Fri","Sat","Sun"];

for(let n=1;n<=12;n++){
 let d=document.createElement('div');
 d.className='number'; d.textContent=n;
 let a=(n*30-90)*Math.PI/180;
 d.style.left=(250+210*Math.cos(a))+'px';
 d.style.top=(250+210*Math.sin(a))+'px';
 nums.appendChild(d);
}

function pad(n,s){return String(n).padStart(s,'0');}

function tick(){
 let now=new Date();
 digital.textContent=`${now.getFullYear()}/${pad(now.getMonth()+1,2)}/${pad(now.getDate(),2)} ${pad(now.getHours(),2)}:${pad(now.getMinutes(),2)}:${pad(now.getSeconds(),2)}:${pad(now.getMilliseconds(),3)}`;
 calendarText.textContent=now.toDateString();

 let ms=now.getMilliseconds();
 let sec=now.getSeconds()+ms/1000;
 let min=now.getMinutes()+sec/60;
 let hr=(now.getHours()%12)+min/60;

 hour.style.transform=`rotate(${hr*30}deg)`;
 minute.style.transform=`rotate(${min*6}deg)`;
 second.style.transform=`rotate(${sec*6}deg)`;
 document.getElementById('ms').style.transform=`rotate(${ms*0.36}deg)`;

 requestAnimationFrame(tick);
}
tick();

function key(y,m,d){return `${y}-${m+1}-${d}`}

function buildCentury(){
 let start=new Date().getFullYear();
 let c=document.getElementById('yearCalendar');
 for(let y=start;y<start+100;y++){
   let yd=document.createElement('div');
   yd.className='year';
   yd.innerHTML=`<h2>${y}</h2>`;
   let monthsWrap=document.createElement('div');
   monthsWrap.className='months';

   for(let m=0;m<12;m++){
     let md=document.createElement('div');
     md.className='month';
     md.innerHTML=`<h4>${months[m]}</h4>`;
     let grid=document.createElement('div');
     grid.className='days';

     days.forEach(x=>{
       let h=document.createElement('div');
       h.className='hdr';
       h.textContent=x;
       grid.appendChild(h);
     });

     let off=(new Date(y,m,1).getDay()+6)%7;
     for(let i=0;i<off;i++) grid.appendChild(document.createElement('div'));

     let dim=new Date(y,m+1,0).getDate();
     for(let d=1;d<=dim;d++){
       let cell=document.createElement('div');
       cell.className='day';
       if(localStorage.getItem('note-'+key(y,m,d))) cell.classList.add('hasNote');
       cell.textContent=d;
       cell.onclick=()=>openNote(y,m,d);
       grid.appendChild(cell);
     }
     md.appendChild(grid);
     monthsWrap.appendChild(md);
   }
   yd.appendChild(monthsWrap);
   c.appendChild(yd);
 }
}
function openNote(y,m,d){
 noteDate.textContent=key(y,m,d);
 noteText.value=localStorage.getItem('note-'+key(y,m,d))||'';
 notePanel.style.display='block';
}
function saveNote(){
 let k=noteDate.textContent;
 if(noteText.value.trim()) localStorage.setItem('note-'+k,noteText.value);
 else localStorage.removeItem('note-'+k);
 notePanel.style.display='none';
}
function closeNote(){notePanel.style.display='none';}
buildCentury();
</script>
</body>
</html>
