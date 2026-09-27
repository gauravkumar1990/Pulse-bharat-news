<script setup>
import {ref,inject,onMounted,onBeforeUnmount} from 'vue';import {Bookmark,Share2,Clock,Play,ChevronRight} from 'lucide-vue-next';
const tab=inject('homeTab'),saved=ref(new Set(JSON.parse(localStorage.getItem('pulseSaved')||'[]'))),visible=ref(8),sentinel=ref(null);let obs;
const news=[
['રાજકોટ','રાજકોટમાં જાહેર પરિવહન માટે નવી યોજના: નવા રૂટ અને ડિજિટલ સુવિધા પર ભાર','થોડી મિનિટ પહેલાં','https://images.unsplash.com/photo-1595658658481-d53d3f999875?auto=format&fit=crop&w=500&q=78'],
['ગુજરાત','ગુજરાતના અનેક વિસ્તારોમાં બદલાયું હવામાન; આગામી દિવસો માટે માર્ગદર્શન','થોડી મિનિટ પહેલાં','https://images.unsplash.com/photo-1519692933481-e162a57d6721?auto=format&fit=crop&w=500&q=78'],
['બિઝનેસ','નાના વેપારીઓમાં ડિજિટલ પેમેન્ટનો ઉપયોગ વધ્યો, નવી સેવાઓથી ગ્રાહકોને ફાયદો','12 મિનિટ પહેલાં','https://images.unsplash.com/photo-1556742049-0cfed4f6a45d?auto=format&fit=crop&w=500&q=78'],
['સ્પોર્ટ્સ','યુવા ખેલાડીઓને નવી તક; આગામી સ્પર્ધા માટે તૈયારીઓ તેજ','18 મિનિટ પહેલાં','https://images.unsplash.com/photo-1540747913346-19e32dc3e97e?auto=format&fit=crop&w=500&q=78'],
['ટેક','AI આધારિત સાધનો રોજિંદા કામ કરવાની રીત ઝડપથી બદલી રહ્યા છે','25 મિનિટ પહેલાં','https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=500&q=78'],
['દેશ','આજના મુખ્ય રાષ્ટ્રીય સમાચાર અને મહત્વની ઘટનાઓ એક નજરમાં','31 મિનિટ પહેલાં','https://images.unsplash.com/photo-1532664189809-02133fee698d?auto=format&fit=crop&w=500&q=78'],
['હેલ્થ','રોજિંદા જીવનમાં સારી ઊંઘ અને સ્વસ્થ આદતો કેમ મહત્વની છે','42 મિનિટ પહેલાં','https://images.unsplash.com/photo-1505751172876-fa1923c5c528?auto=format&fit=crop&w=500&q=78'],
['કરિયર','નવી ટેકનોલોજી સાથે બદલાતી નોકરીઓ માટે કઈ કુશળતા ઉપયોગી?','55 મિનિટ પહેલાં','https://images.unsplash.com/photo-1454165804606-c3d57bc86b40?auto=format&fit=crop&w=500&q=78'],
['ગુજરાત','સ્થાનિક પ્રવાસન સ્થળોમાં સુવિધા વધારવા માટે નવી પહેલ','1 કલાક પહેલાં','https://images.unsplash.com/photo-1524492412937-b28074a5d7da?auto=format&fit=crop&w=500&q=78'],
['બિઝનેસ','નવા ઉદ્યોગો અને સ્ટાર્ટઅપ્સ માટે બજારમાં વધતી તકો','1 કલાક પહેલાં','https://images.unsplash.com/photo-1551836022-d5d88e9218df?auto=format&fit=crop&w=500&q=78'],
['વિશ્વ','દુનિયાભરના આજના મહત્વના સમાચારનો ઝડપી સાર','2 કલાક પહેલાં','https://images.unsplash.com/photo-1521295121783-8a321d551ad2?auto=format&fit=crop&w=500&q=78'],
['લાઇફ','શહેરના વ્યસ્ત જીવનમાં સમય સંચાલન માટે સરળ રીતો','2 કલાક પહેલાં','https://images.unsplash.com/photo-1499750310107-5fef28a66643?auto=format&fit=crop&w=500&q=78']];
const gossip=[
['પલ્સ રિસર્ચ','આંકડા અને તથ્યો પાછળની સંપૂર્ણ કહાની','https://images.unsplash.com/photo-1551836022-d5d88e9218df?auto=format&fit=crop&w=900&q=80'],
['ગ્રાઉન્ડ રિપોર્ટ','શહેર અને ગામમાંથી સીધો વિશેષ અહેવાલ','https://images.unsplash.com/photo-1533130061792-64b345e4a833?auto=format&fit=crop&w=600&q=80'],
['મની એક્સપ્લેનર','રૂપિયો, બજાર અને પૈસાની સરળ સમજ','https://images.unsplash.com/photo-1579621970795-87facc2f976d?auto=format&fit=crop&w=600&q=80'],
['ફિલ્મી ગપશપ','મનોરંજન જગતની રસપ્રદ વાતો','https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?auto=format&fit=crop&w=600&q=80'],
['કરિયર ફંડા','નોકરી અને અભ્યાસની ઉપયોગી માર્ગદર્શિકા','https://images.unsplash.com/photo-1454165804606-c3d57bc86b40?auto=format&fit=crop&w=600&q=80'],
['જાણવું જરૂરી','રોજિંદા જીવન માટે ઉપયોગી સમજ','https://images.unsplash.com/photo-1505751172876-fa1923c5c528?auto=format&fit=crop&w=600&q=80']];
function toggle(i){saved.value.has(i)?saved.value.delete(i):saved.value.add(i);saved.value=new Set(saved.value);localStorage.setItem('pulseSaved',JSON.stringify([...saved.value]))}
function share(n){navigator.share?.({title:n[1],text:n[1],url:location.href})}
onMounted(()=>{obs=new IntersectionObserver(es=>{if(es[0]?.isIntersecting)visible.value=Math.min(visible.value+4,news.length*4)},{rootMargin:'400px'});if(sentinel.value)obs.observe(sentinel.value)});onBeforeUnmount(()=>obs?.disconnect());
function itemAt(i){return news[i%news.length]}
</script>
<template><div class="tab-content">
<section v-if="tab==='top'" class="infinite-news"><div class="feed-heading"><div><i></i><h2>તાજા સમાચાર</h2></div><span>હમણાં અપડેટ</span></div><article v-for="i in visible" :key="'feed'+i" class="infinite-card"><div class="infinite-copy"><h3><em>{{itemAt(i-1)[0]}}:</em> {{itemAt(i-1)[1]}}</h3><small><Clock/> {{itemAt(i-1)[2]}}</small><div><button @click="toggle(i)" :class="{saved:saved.has(i)}"><Bookmark :fill="saved.has(i)?'currentColor':'none'"/> સેવ</button><button @click="share(itemAt(i-1))"><Share2/> શેર</button></div></div><img :src="itemAt(i-1)[3]"></article><div ref="sentinel" class="feed-loader"><span></span><span></span><span></span></div></section>
<section v-else-if="tab==='gossip'" class="gossip-grid"><article v-for="(g,i) in gossip" :class="{hero:i===0}"><img :src="g[2]"><div class="shade"></div><div class="gossip-copy"><span v-if="i===0">PULSE વિશેષ</span><h2>{{g[0]}}</h2><p>{{g[1]}}</p></div></article></section>
<section v-else class="infinite-news local-feed"><div class="feed-heading"><div><i></i><h2>મારું રાજકોટ</h2></div><span>લોકલ અપડેટ</span></div><article v-for="i in 10" class="infinite-card"><div class="infinite-copy"><h3><em>રાજકોટ:</em> {{itemAt(i-1)[1]}}</h3><small><Clock/> {{itemAt(i-1)[2]}}</small><div><button><Bookmark/> સેવ</button><button @click="share(itemAt(i-1))"><Share2/> શેર</button></div></div><img :src="itemAt(i-1)[3]"></article></section>
</div></template>