<script setup>
import {ref} from 'vue';import {Play,Bookmark,Share2,ChevronRight,Download} from 'lucide-vue-next';
const saved=ref(new Set());
const features=[
['પલ્સ રિસર્ચ','આંકડા અને તથ્યો પાછળની સંપૂર્ણ કહાની','https://images.unsplash.com/photo-1551836022-d5d88e9218df?auto=format&fit=crop&w=600&q=75'],
['ગ્રાઉન્ડ રિપોર્ટ','ગામ અને શહેરમાંથી સીધો વિશેષ અહેવાલ','https://images.unsplash.com/photo-1533130061792-64b345e4a833?auto=format&fit=crop&w=600&q=75'],
['મની એક્સપ્લેનર','રૂપિયો, બજાર અને તમારા પૈસાની સરળ સમજ','https://images.unsplash.com/photo-1579621970795-87facc2f976d?auto=format&fit=crop&w=600&q=75'],
['પંથ','ગુજરાતનો વારસો, પ્રવાસ અને લોકસંસ્કૃતિ','https://images.unsplash.com/photo-1524492412937-b28074a5d7da?auto=format&fit=crop&w=600&q=75'],
['જાણવું જરૂરી છે','આરોગ્ય, વિજ્ઞાન અને રોજિંદા જીવનની ઉપયોગી વાત','https://images.unsplash.com/photo-1505751172876-fa1923c5c528?auto=format&fit=crop&w=600&q=75'],
['કરિયર ફંડા','નોકરી, અભ્યાસ અને કારકિર્દીની માર્ગદર્શિકા','https://images.unsplash.com/photo-1454165804606-c3d57bc86b40?auto=format&fit=crop&w=600&q=75']];
const reels=[
['નાગરિક સુવિધામાં નવા ફેરફારની 52 સેકન્ડમાં સંપૂર્ણ વાત','00:52','https://images.unsplash.com/photo-1444723121867-7a241cacace9?auto=format&fit=crop&w=400&q=75'],
['ગુજરાતના હવામાનની આજની ઝડપી અપડેટ','00:39','https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=400&q=75'],
['ટેકનોલોજી જગતના ત્રણ મોટા સમાચાર','00:32','https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=400&q=75'],
['આજની રમતગમતની મોટી ખબર','00:45','https://images.unsplash.com/photo-1540747913346-19e32dc3e97e?auto=format&fit=crop&w=400&q=75']];
const news=[
['રાજકોટમાં જાહેર પરિવહન માટે નવી યોજના: નવા રૂટ અને ડિજિટલ સુવિધા પર ભાર','રાજકોટ','https://images.unsplash.com/photo-1595658658481-d53d3f999875?auto=format&fit=crop&w=400&q=75'],
['ગુજરાતના અનેક વિસ્તારોમાં બદલાયું હવામાન; આગામી દિવસો માટે માર્ગદર્શન','ગુજરાત','https://images.unsplash.com/photo-1519692933481-e162a57d6721?auto=format&fit=crop&w=400&q=75'],
['નાના વેપારીઓમાં ડિજિટલ પેમેન્ટનો ઉપયોગ વધ્યો, નવી સેવાઓથી ગ્રાહકોને ફાયદો','બિઝનેસ','https://images.unsplash.com/photo-1556742049-0cfed4f6a45d?auto=format&fit=crop&w=400&q=75'],
['યુવા ખેલાડીઓને નવી તક; આગામી સ્પર્ધા માટે તૈયારીઓ તેજ','સ્પોર્ટ્સ','https://images.unsplash.com/photo-1540747913346-19e32dc3e97e?auto=format&fit=crop&w=400&q=75']];
function toggle(i){saved.value.has(i)?saved.value.delete(i):saved.value.add(i);saved.value=new Set(saved.value);localStorage.setItem('savedRef',JSON.stringify([...saved.value]))}
</script>
<template><div class="reference-home"><section class="feature-grid"><article v-for="f in features"><img :src="f[2]"><div></div><h2>{{f[0]}}</h2><p>{{f[1]}}</p></article></section><section class="reel-section"><div class="ref-title"><h2>ઝડપી REEL</h2><router-link to="/shorts">બધું જુઓ</router-link></div><div class="reel-row"><router-link v-for="r in reels" to="/shorts" class="reel"><img :src="r[2]"><span class="play"><Play/></span><time>{{r[1]}}</time><b>{{r[0]}}</b></router-link></div></section><section class="latest-section"><div class="ref-title"><h2>તાજા સમાચાર</h2><span>હમણાં અપડેટેડ</span></div><article class="compact-news" v-for="(n,i) in news"><div class="compact-copy"><strong><em>{{n[1]}}:</em> {{n[0]}}</strong><small>થોડી મિનિટ પહેલાં</small><div><button @click="toggle(i)" :class="{active:saved.has(i)}"><Bookmark/> {{saved.has(i)?'સેવ થયું':'સેવ'}}</button><button><Share2/> શેર</button></div></div><img :src="n[2]"></article></section><div class="download-box"><Download/><div><b>ઓફલાઇન વાંચો</b><p>સેવ કરેલા સમાચાર અને એપ શેલ ઇન્ટરનેટ વગર પણ ઉપલબ્ધ રહેશે.</p></div><ChevronRight/></div></div></template>