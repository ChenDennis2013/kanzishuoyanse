<script setup lang="ts">
import { ref } from 'vue';

const score = ref(0);
const colors = ["red", "orange", "yellow",
                "green", "cyan", "blue",
                "purple", "pink", "black"];
const chinese = ["红", "橙", "黄",
                 "绿", "青", "蓝",
                 "紫", "粉", "黑"];
let intv = NaN, timer = NaN;
const time = ref(0);

function rander(seed : number)
{
    let res = seed % 233280;
    return () => {
        res = (res * 9301 + 49297) % 233280;
        return res / 233280;
    }
}
const rnd = rander(Date.now());

function refr()
{
    let ch = chinese[Math.floor(rnd() * 9)], col = colors[Math.floor(rnd() * 9)];
    const q = document.querySelector("#main") as HTMLParagraphElement;
    q.style.color = col; q.innerHTML = ch;
}
function tick()
{
    -- time.value;
    if (!time.value)
    {
        clearInterval(intv);
        clearInterval(timer);
        window.alert("游戏结束，分数：" + String(score.value));
    }
}
function rest()
{
    if (isNaN(intv)) (document.querySelector("#restbtn") as HTMLButtonElement).innerHTML = "重新开始";
    else clearInterval(intv);
    refr(); intv = setInterval(refr, 1500);
    if (!isNaN(timer)) clearInterval(timer);
    time.value = 60; timer = setInterval(tick, 1000);
    score.value = 0;
}
function guess(col : string)
{
    if (!time.value) return; //时间结束不可加分
    const q = document.querySelector("#main") as HTMLParagraphElement;
    if (col == q.style.color) ++ score.value;
    else -- score.value;
    clearInterval(intv);
    refr(); intv = setInterval(refr, 1500);
}
</script>

<template>
  <div style="font-size: 3em;">
    时间：{{ time }}
    &nbsp;&nbsp;&nbsp;
    分数: {{ score }}
    &nbsp;&nbsp;&nbsp;
    <button @click="rest();" id="restbtn" style="font-size: 1em;">开始</button>
  </div>
  <p style="font-size: 8em; margin: 10px;" id="main">&ensp;</p>
  <div style="font-size: 2em;">
    <div>
        <button @click="guess('red');" class="colbtn">红</button>&nbsp;&nbsp;
        <button @click="guess('orange');" class="colbtn">橙</button>&nbsp;&nbsp;
        <button @click="guess('yellow');" class="colbtn">黄</button>
    </div>&ensp;
    <div>
        <button @click="guess('green');" class="colbtn">绿</button>&nbsp;&nbsp;
        <button @click="guess('cyan');" class="colbtn">青</button>&nbsp;&nbsp;
        <button @click="guess('blue');" class="colbtn">蓝</button>
    </div>&ensp;
    <div>
        <button @click="guess('purple');" class="colbtn">紫</button>&nbsp;&nbsp;
        <button @click="guess('pink');" class="colbtn">粉</button>&nbsp;&nbsp;
        <button @click="guess('black');" class="colbtn">黑</button>
    </div>
  </div>
</template>
