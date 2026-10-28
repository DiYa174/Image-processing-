<!DOCTYPE html>
<html lang=”en”><head><meta charset=”utf-8”>
<meta name=”viewport” content=”width=device-width, initial-scale=1, viewport-fit=cover”>
<title>PixelBench – Image Processing Lab</title>
<link rel=”stylesheet”
href=https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@400;600;700&family=
DM+Mono&display=swap>
<style>
:root{--bg:#e8edf1;--card:#fff;--ink:#101b2d;--mute:#55657a;--hot:#e8590c;--line:#cfd8e1;--
film:#111827;--code:#0d1726;box-sizing:border-box;padding-top:env(safe-area-inset-
top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media(prefers-color-scheme:dark){:root:not([data-theme=”light”]){--bg:#0c121b;--card:#141d2a;-
-ink:#e6edf5;--mute:#94a3b8;--hot:#ff8a4c;--line:#263446;--film:#05080d;--code:#070d15;color-
scheme:dark}}
:root[data-theme=”dark”]{--bg:#0c121b;--card:#141d2a;--ink:#e6edf5;--mute:#94a3b8;--
hot:#ff8a4c;--line:#263446;--film:#05080d;--code:#070d15;color-scheme:dark}
Html{scroll-padding-top:env(safe-area-inset-top,0px)}*,*::before,*::after{box-sizing:border-box}
Body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.55 “Bricolage
Grotesque”,system-ui,sans-serif}
.w{max-width:980px;margin:auto;padding:0 16px}
Header .w{display:flex;justify-content:space-between;align-items:center;padding-top:14px}
H1{font-size:clamp(30px,7vw,52px);line-height:1.05;margin:18px 0 8px;letter-spacing:-.02em}
.btn{background:var(--ink);color:var(--bg);border:0;border-radius:8px;padding:10px 16px;font:600
15px inherit;font-family:inherit;cursor:pointer;min-height:44px}
.film{display:flex;gap:10px;overflow-x:auto;background:var(--film);padding:22px
14px;margin:18px 0;border-radius:6px;background:repeating-linear-gradient(90deg,#ffffff40 0
10px,transparent 10px 22px) 0 6px/100% 7px no-repeat,repeating-linear-gradient(90deg,#ffffff40 0
10px,transparent 10px 22px) 0 calc(100% - 6px)/100% 7px no-repeat,var(--film)}
.th{flex:0 0 auto;width:112px;background:none;border:2px solid
transparent;padding:0;color:#cbd5e1;font:13px inherit;font-family:inherit;text-
align:left;cursor:pointer}
.th img{width:100%;aspect-ratio:1;object-fit:cover;display:block}.th[aria-pressed=true]{border-
color:var(--hot)}
.tabs{display:flex;gap:6px;overflow-x:auto;padding:2px 0 12px}
.tabs button{flex:0 0 auto;min-height:44px;padding:0 14px;border-radius:8px;border:1px solid var(--
line);background:var(--card);color:var(--ink);font:600 14px inherit;font-
family:inherit;cursor:pointer}
.tabs button[aria-selected=true]{background:var(--ink);color:var(--bg);border-color:var(--ink)}
.panel{background:var(--card);border:1px solid var(--line);border-radius:8px;padding:18px}
H2{margin:0 0 6px;font-size:24px;letter-spacing:-.01em}.aim{color:var(--mute);margin:0 0
14px;max-width:70ch}
#ctrl{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:12px;margin-
bottom:14px}
#ctrl label{display:flex;flex-direction:column;gap:4px;font-size:14px;color:var(--mute)}
Select{min-height:44px;border-radius:6px;border:1px solid var(--line);background:var(--
bg);color:var(--ink);font:inherit;padding:0 8px}
Input[type=range]{width:100%;min-height:36px;accent-color:var(--hot)}
.io{display:grid;grid-template-columns:1fr 1fr;gap:10px}.io figure{margin:0}.io figcaption{font-
size:13px;color:var(--mute);margin-bottom:4px}
Canvas{width:100%;height:auto;background:#fff;border:1px solid var(--line);border-
radius:4px;display:block}
.tiles{display:flex;flex-wrap:wrap;gap:8px;margin:12px 0}.tiles div{background:var(--bg);border-
radius:6px;padding:6px 12px}.tiles b{display:block;font-size:15px}.tiles span{font-
size:12px;color:var(--mute)}
Pre{background:var(--code);color:#cfe4ff;padding:14px;border-radius:6px;overflow-
x:auto;font:13px/1.5 “DM Mono”,ui-monospace,monospace;margin:14px 0 0}
.chk label{display:flex;gap:12px;align-items:center;padding:11px 0;border-bottom:1px solid var(--
line)}.chk input{width:22px;height:22px;accent-color:var(--hot)}
Footer{padding:26px 0;color:var(--mute);font-size:14px}footer p{margin:2px 0}
:focus-visible{outline:3px solid var(--hot);outline-offset:2px}
</style></head><body>
<header><div class=”w”><strong>PixelBench</strong><button class=”btn” id=”mode”>Dark /
light</button></div></header>
<div class=”w”>
<h1>Twelve experiments, one browser tab.</h1>
<p class=”aim”>Pick a frame from the film strip, choose an experiment, and watch the pixels change.
Everything runs in your browser, with the matching Python and OpenCV program under each
result.</p>
<button class=”btn” id=”upb”>Upload your image</button><input id=”up” type=”file”
accept=”image/*” hidden>
<div class=”film” id=”film” role=”group” aria-label=”Input images”></div>
<div class=”tabs” id=”tabs” role=”tablist” aria-label=”Practicals”></div>
<section class=”panel”><h2 id=”ttl”></h2><p class=”aim” id=”aim”></p>
<div id=”setup” class=”chk” hidden></div>
<div id=”lab”><div id=”ctrl”></div><div
class=”io”><figure><figcaption>Input</figcaption><canvas
id=”inp”></canvas></figure><figure><figcaption>Output</figcaption><canvas
id=”out”></canvas></figure></div><div class=”tiles” id=”tiles”></div></div>
<pre id=”code”></pre></section>
<footer><p>Diya Sandeep Jaiswal, USN CS22136</p><p>Image Processing Lab (N-PECCS502P),
V Semester B.Tech. CSE</p><p>S. B. Jain Institute of Technology, Management &amp; Research,
Nagpur</p></footer></div>
<script>
Const $=s=>document.querySelector(s);let seed=7;const
rnd=()=>(seed=seed*16807%2147483647)/2147483647;
Const mk=f=>{const
c=document.createElement(‘canvas’);c.width=c.height=256;f(c.getContext(‘2d’));return c};
Const circ=(x,c,a,b,r)=>{x.fillStyle=c;x.beginPath();x.arc(a,b,r,0,7);x.fill()};
Function scene(x){const
g=x.createLinearGradient(0,0,0,256);g.addColorStop(0,’#38bdf8’);g.addColorStop(1,’#e0f2fe’);x.fil
lStyle=g;x.fillRect(0,0,256,256);
Circ(x,’#fde047’,200,52,28);circ(x,’#22c55e’,60,300,150);circ(x,’#16a34a’,230,310,130);
x.fillStyle=’#f97316’;x.fillRect(90,140,80,70);x.fillStyle=’#dc2626’;x.beginPath();x.moveTo(80,14
2);x.lineTo(130,100);x.lineTo(180,142);x.fill();
x.fillStyle=’#fff’;x.fillRect(116,170,24,40);x.fillStyle=’#0ea5e9’;x.fillRect(98,152,16,16);
[[40,50],[90,40],[150,70]].forEach(([a,b])=>{circ(x,’#fff’,a,b,14);circ(x,’#fff’,a+16,b+3,11);circ(x,’
#fff’,a-14,b+4,10)})}
Const SRC=[mk(scene),
Mk(x=>{scene(x);x.strokeStyle=’#000’;x.lineWidth=7;x.lineCap=’round’;[[10,20,240,240],[240,30,
20,230],[10,120,250,150],[130,0,90,256]].forEach(([a,b,c,d])=>{x.beginPath();x.moveTo(a,b);x.line
To(c,d);x.stroke()})}),
Mk(x=>{x.fillStyle=’#fff’;x.fillRect(0,0,256,256);x.fillStyle=’#000’;x.font=’bold 210px
Georgia,serif’;x.textAlign=’center’;x.textBaseline=’middle’;x.fillText(‘N’,128,138);for(let
i=0;i<260;i++){x.fillStyle=rnd()<.5?’#000’:’#fff’;x.fillRect(rnd()*256|0,rnd()*256|0,rnd()<.8?1:2,rn
d()<.8?1:2)}}),
Mk(x=>{x.fillStyle=’#fff’;x.fillRect(0,0,256,256);circ(x,’#ef4444’,70,70,32);x.fillStyle=’#3b82f6’;
x.fillRect(140,40,64,64);x.fillStyle=’#22c55e’;x.beginPath();x.moveTo(50,235);x.lineTo(110,155);x.
lineTo(170,235);x.fill();circ(x,’#eab308’,205,190,36);x.fillStyle=’#a855f7’;x.fillRect(20,120,60,20)
})];
Const NAMES=[‘Colour scene’,’Damaged scene’,’Binary letter’,’Shapes’];
Const gd=c=>c.getContext(‘2d’).getImageData(0,0,c.width,c.height);
Const gray=d=>{const n=d.width*d.height,g=new Float32Array(n),p=d.data;for(let
i=0;i<n;i++)g[i]=.299*p[4*i]+.587*p[4*i+1]+.114*p[4*i+2];return g};
Const G2I=(g,w,h)=>{const d=new ImageData(w,h);for(let
i=0;i<w*h;i++){d.data[4*i]=d.data[4*i+1]=d.data[4*i+2]=g[i];d.data[4*i+3]=255}return d};
Const chans=d=>[0,1,2].map(k=>{const a=new Float32Array(d.width*d.height);for(let
i=0;i<a.length;i++)a[i]=d.data[4*i+k];return a});
Const merge=(ch,w,h)=>{const d=new ImageData(w,h);for(let i=0;i<w*h;i++){for(let
k=0;k<3;k++)d.data[4*i+k]=ch[k][i];d.data[4*i+3]=255}return d};
Const per=(d,f)=>merge(chans(d).map(a=>f(a,d.width,d.height)),d.width,d.height);
Const cl=(v,m)=>v<0?0:v>=m?m-1:v;
Function conv(a,w,h,k,n){const o=new Float32Array(a.length),r=n>>1;for(let y=0;y<h;y++)for(let
x=0;x<w;x++){let s=0,j=0;for(let v=-r;v<=r;v++)for(let u=-
r;u<=r;u++)s+=a[cl(y+v,h)*w+cl(x+u,w)]*k[j++];o[y*w+x]=s}return o}
Const box=n=>new Float32Array(n*n).fill(1/(n*n));
Const gk=n=>{const s=.3*((n-1)/2-1)+.8,k=[];let t=0;for(let v=0;v<n;v++)for(let
u=0;u<n;u++){const e=Math.exp(-((u-(n-1)/2)**2+(v-(n-1)/2)**2)/(2*s*s));k.push€;t+=e}return
k.map(e=>e/t)};
Function med(a,w,h,n){const o=new Float32Array(a.length),r=n>>1,b=[];for(let y=0;y<h;y++)for(let
x=0;x<w;x++){b.length=0;for(let v=-r;v<=r;v++)for(let u=-
r;u<=r;u++)b.push(a[cl(y+v,h)*w+cl(x+u,w)]);b.sort((p,q)=>p-q);o[y*w+x]=b[b.length>>1]}return
o}
Function bil(a,w,h,n){const o=new Float32Array(a.length),r=n>>1,ss=r||1;for(let
y=0;y<h;y++)for(let x=0;x<w;x++){const c=a[y*w+x];let s=0,t=0;for(let v=-r;v<=r;v++)for(let u=-
r;u<=r;u++){const q=a[cl(y+v,h)*w+cl(x+u,w)],wt=Math.exp(-(u*u+v*v)/(2*ss*ss)-(q-
c)**2/3200);s+=q*wt;t+=wt}o[y*w+x]=s/t}return o}
Const KX={Sobel:[-1,0,1,-2,0,2,-1,0,1],Prewitt:[-1,0,1,-1,0,1,-1,0,1]},KY={Sobel:[-1,-2,-
1,0,0,0,1,2,1],Prewitt:[-1,-1,-1,0,0,0,1,1,1]};
Const sel=(k,l,o)=>({k,l,o}),rng=(k,l,min,max,v)=>({k,l,min,max,v});
Const cvs=(w,h)=>{const o=document.createElement(‘canvas’);o.width=w;o.height=h;return o};
Function mo(f,w,h,n,e){const r=n>>1,o=new Uint8Array(w*h);for(let y=0;y<h;y++)for(let
x=0;x<w;x++){let res=e?1:0;for(let v=-r;v<=r;v++)for(let u=-r;u<=r;u++){const
X=x+u,Y=y+v,b=X>=0&&Y>=0&&X<w&&Y<h?f[Y*w+X]:0;if(e?!b:b)res=e?0:1}o[y*w+x]=res}
return o}
Function cs(sp,r,g,b){if(sp==’RGB’)return[r,g,b];
If(sp==’HSV’){const M=Math.max(r,g,b),m=Math.min(r,g,b),d=M-m;let h=0;if(d){h=M==r?((g-
b)/d+6)%6:M==g?(b-r)/d+2r-g)/d+4}return[h*42.5,M?d/M*255:0,M]}
If(sp==’YCrCb’){const Y=.299*r+.587*g+.114*b;return[Y,(r-Y)*.713+128,(b-Y)*.564+128]}
Const lin=v=>{v/=255;return
v>.04045?((v+.055)/1.055)**2.4:v/12.92},f=t=>t>.008856?Math.cbrt(t):7.787*t+16/116,R=lin®,G=
lin(g),B=lin(b);
Const
fx=f((.4124*R+.3576*G+.1805*B)/.95047),fy=f(.2126*R+.7152*G+.0722*B),fz=f((.0193*R+.1192
*G+.9505*B)/1.08883);return[(116*fy-16)*2.55,500*(fx-fy)+128,200*(fy-fz)+128]}
Let info=[];
Const P=[
{n:’Setup’,t:’Python and Development Environment Setup’,src:0,c:[],aim:’Prelab: introduction to
Python and setup of the development environment (necessary tools and libraries) for image
processing, including the PyCharm IDE, Jupyter, and installation of OpenCV, NumPy, pandas, scikit-
learn and matplotlib.’,
Code:`pip install opencv-python numpy pandas scikit-learn matplotlib jupyter\n\nimport cv2, numpy,
pandas, sklearn, matplotlib\nprint(cv2.__version__, numpy.__version__)`},
{n:’Formats’,t:’Image Format Conversion, Arithmetic and Bitwise Operations’,src:0,aim:’To convert
images between various formats like RGB and Grayscale, perform arithmetic and bitwise operations
on the images, and observe how these operations affect the image data representation.’,
C:[sel(‘op’,’Operation’,[‘RGB to Grayscale’,’Add constant’,’Subtract constant’,’Bitwise
NOT’,’Bitwise AND (circle mask)’,’Bitwise XOR (circle mask)’]),rng(‘a’,’Value
(arithmetic)’,0,200,60)],
Run(c,p){const d=gd©,w=d.width,h=d.height,q=d.data;if(p.op.includes(‘Gray’))return
G2I(gray(d),w,h);
For(let i=0;i<w*h;i++){const x=i%w,y=i/w|0,m=(x-w/2)**2+(y-h/2)**2<(h*.39)**2?255:0;for(let
k=0;k<3;k++){const v=q[4*i+k];q[4*i+k]=p.op.includes(‘Add’)?v+p.a:p.op.includes(‘Sub’)?v-
p.a:p.op.includes(‘NOT’)?255-v:p.op.includes(‘AND’)?v&m:v^m}}
Info=[[‘Size’,w+’ × ‘+h],[‘Operation’,p.op]];return d},
Code:`import cv2\nimg = cv2.imread(“input.jpg”)\ngray = cv2.cvtColor(img,
cv2.COLOR_BGR2GRAY)\nadded = cv2.add(img, 60)\ninv = cv2.bitwise_not(img)\nand_ =
cv2.bitwise_and(img, img, mask=mask)`},
{n:’Geometry’,t:’2-D Geometric Transformations’,src:0,aim:’Develop programs to apply the
following 2-D geometric transformation operations on an image: translation, rotation, scaling,
shearing, reflection and cropping.’,
C:[sel(‘op’,’Transformation’,[‘Translation’,’Rotation’,’Scaling’,’Shearing’,’Reflection’,’Cropping’]
),rng(‘a’,’Value 1’,-100,100,30),rng(‘b’,’Value 2’,-100,100,30)],
Run(c,p){const w=c.width,h=c.height,a=p.a,b=p.b;let e=’’;
If(p.op==’Cropping’){const s=1-
Math.abs(a)*.008,sw=w*s|0,sh=h*s|0,o=cvs(sw,sh);o.getContext(‘2d’).drawImage(c,(w-sw)/2,(h-
sh)/2,sw,sh,0,0,sw,sh);info=[[‘Transformation’,p.op],[‘Kept’,(s*100|0)+’%’]];return o}
Const o=cvs(w,h),x=o.getContext(‘2d’);x.fillStyle=’#fff’;x.fillRect(0,0,w,h);
If(p.op==’Translation’){x.translate(a,b);e=a+’, ‘+b+’ px’}
Else if(p.op==’Rotation’){x.translate(w/2,h/2);x.rotate(a*1.8*Math.PI/180);x.translate(-w/2,-
h/2);e=a*1.8+’°’}
Else if(p.op==’Scaling’){const s=1+a/100;x.translate(w/2,h/2);x.scale(s,s);x.translate(-w/2,-
h/2);e=’×’+s.toFixed(2)}
Else if(p.op==’Shearing’){x.transform(1,b/100,a/100,1,-a/100*h/2,-b/100*w/2);e=a/100+’,
‘+b/100}
Else{const m=a<-33?’horizontal’:a>33?’vertical’:’both axes’;if(m!=’vertical’)x.setTransform(-
1,0,0,1,w,0);if(m==’vertical’)x.setTransform(1,0,0,-1,0,h);if(m==’both axes’)x.setTransform(-1,0,0,-
1,w,h);e=m}
x.drawImage(c,0,0);info=[[‘Transformation’,p.op],[‘Effect’,e]];return o},
code:`import cv2\nh, w = img.shape[:2]\nM = cv2.getRotationMatrix2D((w/2, h/2), 30, 1)\nrot =
cv2.warpAffine(img, M, (w, h))\nT = np.float32([[1,0,30],[0,1,30]])\nflip = cv2.flip(img, 1)\ncrop =
img[50:200, 50:200]`},
{n:’Enhancement’,t:’Spatial Domain Image Enhancement’,src:0,aim:’To study and implement image
enhancement techniques in the spatial domain, including histogram equalization for contrast
improvement, spatial filtering methods such as smoothing and sharpening to reduce noise and enhance
important features, and thresholding techniques to segment grayscale images based on pixel
intensity.’,
C:[sel(‘op’,’Enhancement’,[‘Histogram equalization’,’Smoothing (Gaussian
5×5)’,’Sharpening’,’Thresholding’]),rng(‘a’,’Threshold’,0,255,128)],
Run(c,p){const
d=gd©,w=d.width,h=d.height;info=[[‘Technique’,p.op],[‘Threshold’,p.op==’Thresholding’?p.a:’not
used’]];
If(p.op[0]==’S’&&p.op[1]==’m’)return per(d,(a,w,h)=>conv(a,w,h,gk(5),5));
If(p.op==’Sharpening’)return per(d,(a,w,h)=>conv(a,w,h,[0,-1,0,-1,5,-1,0,-1,0],3));
Const g=gray(d).map(Math.round);if(p.op==’Thresholding’)return
G2I(g.map(v=>v>p.a?255:0),w,h);
Const n=g.length,hs=new Array(256).fill(0);g.forEach(v=>hs[v]++);let s=0;const
lut=hs.map(v=>(s+=v)),c0=lut.find(v=>v>0);return G2I(g.map(v=>(lut[v]-c0)/(n-c0||1)*255),w,h)},
Code:`import cv2\ngray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)\neq =
cv2.equalizeHist(gray)\nblur = cv2.GaussianBlur(img, (5,5), 0)\nsharp = cv2.filter2D(img, -1,
kernel)\n_, th = cv2.threshold(gray, 128, 255, cv2.THRESH_BINARY)`},
{n:’Filters’,t:’Spatial Domain Filters’,src:0,aim:’To write Python programs using OpenCV to apply
different spatial domain filters on a given image, including averaging, Gaussian, median and bilateral
filters.’,
C:[sel(‘op’,’Filter’,[‘Averaging’,’Gaussian’,’Median’,’Bilateral’]),sel(‘k’,’Kernel’,[‘3’,’5’,’7’])],
Run(c,p){const n=+p.k,d=gd©;info=[[‘Filter’,p.op],[‘Kernel’,n+’ × ‘+n]];
Return
per(d,(a,w,h)=>p.op==’Averaging’?conv(a,w,h,box(n),n):p.op==’Gaussian’?conv(a,w,h,gk(n),n):p.o
p==’Median’?med(a,w,h,n):bil(a,w,h,n))},
Code:`import cv2\navg = cv2.blur(img, (5,5))\ngauss = cv2.GaussianBlur(img, (5,5), 0)\nmedian =
cv2.medianBlur(img, 5)\nbilateral = cv2.bilateralFilter(img, 9, 75, 75)`},
{n:’Inpainting’,t:’Image Restoration and Inpainting’,src:1,aim:’Implement removal of damaged parts
of an image using inpainting methods. The experiment uses two approaches, the Telea method and the
Navier-Stokes (NS) method, to restore the image and make it look natural.’,
C:[sel(‘op’,’Method’,[‘Telea-style’,’Navier-Stokes-style’]),rng(‘a’,’Repair
radius’,1,5,3),sel(‘v’,’View’,[‘Restored’,’Damaged mask’])],
Run(c,p){const d=gd©,w=d.width,h=d.height,q=d.data;let m=new Uint8Array(w*h);for(let
i=0;i<w*h;i++)if(Math.max(q[4*i],q[4*i+1],q[4*i+2])<90)m[i]=1;
For(let t=0;t<2;t++){const n=m.slice();for(let y=0;y<h;y++)for(let
x=0;x<w;x++)if(m[y*w+x])for(let v=-1;v<=1;v++)for(let u=-1;u<=1;u++){const
X=x+u,Y=y+v;if(X>=0&&Y>=0&&X<w&&Y<h)n[Y*w+X]=1}m=n}
Let cnt=0;m.forEach(v=>cnt+=v);info=[[‘Method’,p.op],[‘Radius’,p.a],[‘Masked pixels’,cnt]];
If(p.v[0]==’D’)return G2I(m.map(v=>v*255),w,h);
Const o=new Uint8ClampedArray(q),k=m.map(v=>1-v),R=p.a;let left=1,it=0;
While(left&&it++<80){left=0;const nk=k.slice();for(let y=0;y<h;y++)for(let x=0;x<w;x++){const
i=y*w+x;if(k[i])continue;const s=[0,0,0];let t=0;
For(let v=-R;v<=R;v++)for(let u=-R;u<=R;u++){const
X=x+u,Y=y+v;if(X<0||Y<0||X>=w||Y>=h||!k[Y*w+X])continue;const
wt=p.op[0]==’T’?1/(u*u+v*v+1):1;for(let j=0;j<3;j++)s[j]+=o[4*(Y*w+X)+j]*wt;t+=wt}
If(t){for(let j=0;j<3;j++)o[4*i+j]=s[j]/t;nk[i]=1}else left=1}k.set(nk)}
If(p.op[0]==’N’)for(let r=0;r<R*3;r++){const c2=new Uint8ClampedArray(o);for(let y=1;y<h-
1;y++)for(let x=1;x<w-1;x++){const i=y*w+x;if(m[i])for(let j=0;j<3;j++)o[4*i+j]=(c2[4*(i-
1)+j]+c2[4*(i+1)+j]+c2[4*(i-w)+j]+c2[4*(i+w)+j])/4}}
Return new ImageData(o,w,h)},
Code:`import cv2\nmask = cv2.imread(“mask.png”, 0)\ntelea = cv2.inpaint(img, mask, 3,
cv2.INPAINT_TELEA)\nns = cv2.inpaint(img, mask, 3, cv2.INPAINT_NS)`},
{n:’Compression’,t:’Lossless Image Compression’,src:0,aim:’Implement a coding technique to
achieve lossless compression and compare the original and compressed file sizes.’,
C:[sel(‘op’,’Coding technique’,[‘Run-length encoding (RLE)’,’Huffman coding’])],
Run(c,p){const d=gd©,w=d.width,h=d.height,g=gray(d).map(Math.round),n=g.length,runs=[];
For(let i=0;i<n;){let j=I;while(j<n&&g[j]==g[i]&&j-i<255)j++;runs.push([g[i],j-i]);i=j}
Const dec=[];runs.forEach(([v,l])=>{for(let k=0;k<l;k++)dec.push(v)});const
ok=dec.every((v,i)=>v==g[i]);
Const f=Array(256).fill(0);g.forEach(v=>f[v]++);let
gp=f.map((w,s)=>({w,s:[s]})).filter(x=>x.w);const len={};gp.forEach(x=>len[x.s[0]]=0);
While(gp.length>1){gp.sort((a,b)=>a.w-b.w);const
a=gp.shift(),b=gp.shift(),s=[…a.s,…b.s];s.forEach(v=>len[v]++);gp.push({w:a.w+b.w,s})}
Let bits=0;f.forEach((c,s)=>{if©bits+=c*(len[s]||1)});
Const sz=p.op[0]==’R’?runs.length*2:Math.ceil(bits/8)+256;
Info=[[‘Original’,n+’ B’],[‘Compressed’,sz+’ B’],[‘Ratio’,(n/sz).toFixed(2)+’ : 1’],[‘Space
saved’,((1-sz/n)*100).toFixed(1)+’%’],[‘Lossless check’,ok?’Passed’:’Failed’]];return G2I(g,w,h)},
Code:`from collections import Counter\npixels = list(gray.flatten())\nruns, I = [], 0\nwhile I <
len(pixels):\n j = i\n while j < len(pixels) and pixels[j] == pixels[i]: j += 1\n runs.append((pixels[i],
j – i)); I = j`},
{n:’Morphology’,t:’Mathematical Morphology’,src:2,aim:’Perform morphological operations
erosion, dilation, opening and closing on binary images to study their effects on object shapes and
noise removal.’,
C:[sel(‘op’,’Operation’,[‘Erosion’,’Dilation’,’Opening’,’Closing’]),sel(‘k’,’Kernel
size’,[‘3’,’5’,’7’])],
Run(c,p){const d=gd©,w=d.width,h=d.height,g=gray(d),n=+p.k,f=g.map(v=>v<128?1:0);let r;
If(p.op==’Erosion’)r=mo(f,w,h,n,1);else if(p.op==’Dilation’)r=mo(f,w,h,n,0);else
if(p.op==’Opening’)r=mo(mo(f,w,h,n,1),w,h,n,0);else r=mo(mo(f,w,h,n,0),w,h,n,1);
Info=[[‘Operation’,p.op],[‘Structuring element’,n+’ × ‘+n]];return G2I(r.map(v=>v?0:255),w,h)},
Code:`import cv2\nkernel = cv2.getStructuringElement(cv2.MORPH_RECT, (3,3))\nerosion =
cv2.erode(binary, kernel)\ndilation = cv2.dilate(binary, kernel)\nopening =
cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)\nclosing = cv2.morphologyEx(binary,
cv2.MORPH_CLOSE, kernel)`},
{n:’Correlation’,t:’Object Detection by Correlation’,src:3,aim:’Develop a program to detect an object
using the correlation principle.’,
C:[rng(‘a’,’Template X’,0,216,44),rng(‘b’,’Template Y’,0,216,44),rng(‘z’,’Noise’,0,60,20)],
Run(c,p){const d=gd©,w=d.width,h=d.height,T=40,tx=Math.min(p.a,w-T),ty=Math.min(p.b,h-
T),g0=gray(d),t=[];let tm=0;
For(let j=0;j<T;j++)for(let i=0;i<T;i++){const
v=g0[(ty+j)*w+tx+i];t.push(v);tm+=v}tm/=T*T;const tz=t.map(v=>v-
tm),tt=tz.reduce((s,v)=>s+v*v,0)||1;
Const q=d.data;for(let i=0;i<w*h;i++){const nz=(rnd()-.5)*2*p.z;for(let k=0;k<3;k++)q[4*i+k]
