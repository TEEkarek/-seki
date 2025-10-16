let pendingMobs=[];
onWorldAttemptSpawnMob=(mobType,x,y,z)=>{
if(mobType=="Cow"){
const a=api.attemptSpawnMob("Wildcat",x,y+20,z);
pendingMobs.push({id:a,ticksLeft:2,exploded:false});
return"preventSpawn"
}
}
tick=(ms)=>{
for(let i=pendingMobs.length-1;i>=0;i--){
const m=pendingMobs[i];
if(m.ticksLeft>0){
m.ticksLeft--;
if(m.ticksLeft<=0){
api.updateEntityNodeMeshAttachment(m.id,"HeadMesh","BloxdBlock",{blockName:"Fireball Block",size:2,meshOffset:[0,0,0]},[0,0,0],[0,0,0]);

}
}
if(!m.exploded){
try{
let pos=api.getPosition(m.id);
let fractionalY=pos[1]-Math.floor(pos[1]);
if(fractionalY<=0){
api.despawnMob(m.id);
m.exploded=true;
const x=Math.floor(pos[0]);
const y=Math.floor(pos[1]);
const z=Math.floor(pos[2]);
for(let dx=-7;dx<=7;dx++){
for(let dy=-7;dy<=7;dy++){
for(let dz=-7;dz<=7;dz++){
if(dx*dx+dy*dy+dz*dz<=25){
api.setBlock(x+dx,y+dy,z+dz,"Air");
}}}
}
pendingMobs.splice(i,1);
}
}catch(e){}
}
}
}
