<script setup>
import {ref} from 'vue';import {Bookmark,Share2,Play,ChevronRight,Download,Flame,MapPin,Clock} from 'lucide-vue-next';
const saved=ref(new Set(JSON.parse(localStorage.getItem('savedHomeV4')||'[]')));
const sections=[
{title:'ટોપ ન્યૂઝ',icon:'hot',items:[
['ગુજરાત','ગુજરાતના શહેરોમાં નવી વિકાસ યોજનાઓ; પરિવહન અને જાહેર સુવિધા પર ખાસ ધ્યાન','12 મિનિટ પહેલાં','https://images.unsplash.com/photo-1524492412937-b28074a5d7da?auto=format&fit=crop&w=500&q=78'],
['દેશ','આજના મુખ્ય રાષ્ટ્રીય સમાચાર: દિવસભરની મહત્વની ઘટનાઓ એક નજરમાં','20 મિનિટ પહેલાં','https://images.unsplash.com/photo-1532664189809-02133fee698d?auto=format&fit=crop&w=500&q=78']]},
{title:'મારું રાજકોટ',icon:'local',items:[
['રાજકોટ','શહેરના નવા જાહેર પરિવહન રૂટ માટે તૈયારી; મુસાફરોને મળશે ડિજિટલ માહિતી','25 મિનિટ પહેલાં','https://images.unsplash.com/photo-1595658658481-d53d3f999875?auto=format&fit=crop&w=500&q=78'],
['રાજકોટ','સ્થાનિક બજાર અને મુખ્ય માર્ગો માટે નવી સુવિધાઓની કામગીરી શરૂ','38 મિનિટ પહેલાં','https://images.unsplash.com/photo-1480714378408-67cf0d13bc1b?auto=format&fit=crop&w=500&q=78']]},
{title:'ઓરિજિનલ',icon:'original',items:[
['પલ્સ રિસર્ચ','આંકડા અને તથ્યો સાથે વિશેષ વિશ્લેષણ: બદલાતા ગુજરાતની સંપૂર્ણ તસવીર','45 મિનિટ પહેલાં','https://images.unsplash.com/photo-1551836022-d5d88e9218df?auto=format&fit=crop&w=500&q=78'],
['ગ્રાઉન્ડ રિપોર્ટ','લોકો સુધી પહોંચીને તૈયાર કરેલો વિશેષ અહેવાલ: મુદ્દાની જમીની હકીકત','1 કલાક પહેલાં','https://images.unsplash.com/photo-1533130061792-64b345e4a833?auto=format&fit=crop&w=500&q=78']]},
{title:'બિઝનેસ અને ટેક',icon:'biz',items:[
['બિઝનેસ','નાના વેપારીઓમાં ડિજિટલ પેમેન્ટનો ઉપયોગ ઝડપથી વધ્યો','1 કલાક પહેલાં','https://images.unsplash.com/photo-1556742049-0cfed4f6a45d?auto=format&fit=crop&w=500&q=78'],
['ટેક','AI આધારિત સાધનો રોજિંદા કામ કરવાની રીત કેવી રીતે બદલી રહ્યા છે?','2 કલાક પહેલાં','https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=500&q=78']]},
{title:'સ્પોર્ટ્સ',icon:'sport',items:[
['ક્રિકેટ','યુવા ખેલાડીઓને નવી તક; આગામી સ્પર્ધા માટે ટીમની તૈયારીઓ તેજ','2 કલાક પહેલાં','https://images.unsplash.com/photo-1540747913346-19e32dc3e97e?auto=format&fit=crop&w=500&q=78']]}];
const reels=[
['ગુજરાતની આજની મોટી ખબર 60 સેકન્ડમાં','00:52','https://images.unsplash.com/photo-1444723121867-7a241cacace9?auto=format&fit=crop&w=400&q=75'],
['હવામાનની ઝડપી અપડેટ','00:39','https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=400&q=75'],
['ટેકનોલોજીના ત્રણ મોટા સમાચાર','00:32','https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=400&q=75']];
function toggle(key){saved.value.has(key)?saved.value.delete(key):saved.value.add(key);saved.value=new Set(saved.value);localStorage.setItem('savedHomeV4',JSON.stringify([...saved.value]))}
function share(title){navigator.share?.({title,text:title,url:location.href})}
</script>
<template><div class="unified-home"><div class="home-chips"><button class="active">તમારા માટે</button><button>રાજકોટ</button><button>ગુજરાત</button><button>દેશ</button><button>બિઝનેસ</button><button>સ્પોર્ટ્સ</button></div><section v-for="(section,si) in sections" :key="section.title" class="unified-section"><div class="unified-title"><div><Flame v-if="section.icon==='hot'"/><MapPin v-else-if="section.icon==='local'"/><span v-else class="title-dot"></span><h2>{{section.title}}</h2></div><button>બધા જુઓ <ChevronRight/></button></div><article v-for="(n,ni) in section.items" :key="n[1]" class="unified-news-card"><div class="unified-copy"><span class="unified-category" :class="'tone-'+si">{{n[0]}}</span><h3>{{n[1]}}</h3><small><Clock/> {{n[2]}}</small><div class="unified-actions"><button @click="toggle(si+'-'+ni)" :class="{active:saved.has(si+'-'+ni)}"><Bookmark :fill="saved.has(si+'-'+ni)?'currentColor':'none'"/> {{saved.has(si+'-'+ni)?'સેવ થયું':'સેવ'}}</button><button @click="share(n[1])"><Share2/> શેર</button></div></div><img :src="n[3]"></article></section><section class="unified-section reel-card-section"><div class="unified-title"><div><Play/><h2>ઝડપી REEL</h2></div><router-link to="/shorts">બધા જુઓ <ChevronRight/></router-link></div><div class="unified-reels"><router-link v-for="r in reels" to="/shorts"><div><img :src="r[2]"><i><Play/></i><time>{{r[1]}}</time></div><b>{{r[0]}}</b></router-link></div></section><div class="unified-offline"><Download/><div><b>ઓફલાઇન સમાચાર તૈયાર</b><p>વાંચેલા અને સેવ કરેલા સમાચાર ઇન્ટરનેટ વગર પણ ઉપલબ્ધ રહેશે.</p></div><ChevronRight/></div></div></template>