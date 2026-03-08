# pro1
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Recommendation System</title>

<style>

body{
font-family:Arial;
margin:0;
background:#f4f6f9;
}

/* LOGIN */

#loginPage{
display:flex;
flex-direction:column;
align-items:center;
justify-content:center;
height:100vh;
background:linear-gradient(120deg,#4facfe,#00f2fe);
color:white;
}

input,select{
padding:10px;
margin:10px;
border:none;
border-radius:5px;
}

button{
padding:10px 15px;
border:none;
border-radius:5px;
background:#2c7be5;
color:white;
cursor:pointer;
}

button:hover{
background:#1a5edb;
}

/* MAIN */

#mainPage{display:none;padding:20px}

header{
display:flex;
justify-content:space-between;
align-items:center;
background:white;
padding:15px;
box-shadow:0 2px 5px rgba(0,0,0,0.1);
}

.products{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
gap:20px;
margin-top:20px;
}

.card{
background:white;
border-radius:10px;
box-shadow:0 2px 10px rgba(0,0,0,0.1);
padding:15px;
transition:0.3s;
}

.card:hover{
transform:scale(1.03);
}

.card img{
width:100%;
height:150px;
object-fit:cover;
border-radius:5px;
}

.price{
color:green;
font-weight:bold;
}

.shop{
background:#f1f1f1;
padding:5px;
margin:5px 0;
border-radius:5px;
}

/* ADMIN */

#adminPage{
display:none;
padding:20px;
}

.adminBox{
background:white;
padding:20px;
border-radius:10px;
width:400px;
box-shadow:0 2px 10px rgba(0,0,0,0.2);
}

.filters button{
margin:5px;
background:#555;
}

.profile{
background:#fff;
padding:10px;
border-radius:8px;
}

</style>

</head>

<body>

<!-- LOGIN -->

<div id="loginPage">

<h1>Smart Product Finder</h1>

<input id="mobile" placeholder="Enter Mobile Number">

<select id="location">
<option>Karur</option>
<option>Trichy</option>
<option>Salem</option>
<option>Coimbatore</option>
</select>

<select id="role">
<option>User</option>
<option>Admin</option>
</select>

<button onclick="login()">Login</button>

</div>

<!-- MAIN USER PAGE -->

<div id="mainPage">

<header>

<div>
<input id="searchInput" placeholder="Search phone, face wash, hair oil">
<button onclick="searchProduct()">Search</button>
</div>

<div class="profile">
User: <span id="userMobile"></span> |
Location: <span id="userLocation"></span>
</div>

</header>

<div class="filters" id="dynamicFilters"></div>

<div class="products" id="productList"></div>

</div>

<!-- ADMIN PAGE -->

<div id="adminPage">

<h2>Admin Dashboard</h2>

<div class="adminBox">

<h3>Add Product</h3>

<input id="pname" placeholder="Product Name">
<input id="ptype" placeholder="Type (mobile/skin/hair)">
<input id="pbrand" placeholder="Brand">
<input id="pimage" placeholder="Image URL">
<input id="pamz" placeholder="Amazon Price">
<input id="pflp" placeholder="Flipkart Price">
<input id="shopname" placeholder="Local Shop Name">
<input id="shopprice" placeholder="Shop Price">

<button onclick="addProduct()">Add Product</button>

</div>

</div>

<script>

/* PRODUCTS */

let products=[

{
name:"iPhone 14",
type:"mobile",
brand:"Apple",
color:"Black",
image:"https://images.unsplash.com/photo-1511707171634",
rating:"4.8",
review:"Best camera phone",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:78999,flipkart:78499},
shops:[{name:"Reliance Digital",price:79000,map:"https://maps.google.com"}]
},

{
name:"Samsung Galaxy S23",
type:"mobile",
brand:"Samsung",
color:"White",
image:"https://images.unsplash.com/photo-1580910051074",
rating:"4.7",
review:"Flagship performance",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:70999,flipkart:69999},
shops:[{name:"Mobile World",price:70500,map:"https://maps.google.com"}]
},

{
name:"OnePlus 11",
type:"mobile",
brand:"OnePlus",
color:"Black",
image:"https://images.unsplash.com/photo-1603898037225",
rating:"4.6",
review:"Fast performance",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:55999,flipkart:54999},
shops:[{name:"Poorvika Mobiles",price:54000,map:"https://maps.google.com"}]
},

{
name:"Nivea Face Wash",
type:"skin",
skin:"Oily",
image:"https://images.unsplash.com/photo-1596755389378",
rating:"4.4",
review:"Best for oily skin",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:249,flipkart:239},
shops:[{name:"Apollo Pharmacy",price:240,map:"https://maps.google.com"}]
},

{
name:"Himalaya Face Wash",
type:"skin",
skin:"Dry",
image:"https://images.unsplash.com/photo-1608248597279",
rating:"4.3",
review:"Natural herbal wash",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:199,flipkart:189},
shops:[{name:"Medical Store",price:185,map:"https://maps.google.com"}]
},

{
name:"Parachute Hair Oil",
type:"hair",
hair:"Normal",
image:"https://images.unsplash.com/photo-1620912189868",
rating:"4.5",
review:"Healthy hair oil",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:180,flipkart:175},
shops:[{name:"Super Market",price:170,map:"https://maps.google.com"}]
}

]

/* LOGIN */

function login(){

let mobile=document.getElementById("mobile").value
let location=document.getElementById("location").value
let role=document.getElementById("role").value

if(role=="Admin"){
document.getElementById("loginPage").style.display="none"
document.getElementById("adminPage").style.display="block"
}
else{
document.getElementById("loginPage").style.display="none"
document.getElementById("mainPage").style.display="block"

document.getElementById("userMobile").innerText=mobile
document.getElementById("userLocation").innerText=location

displayProducts(products)
}

}

/* DISPLAY */

function displayProducts(list){

let html=""

list.forEach(p=>{

html+=`

<div class="card">

<img src="${p.image}">

<h3>${p.name}</h3>

⭐ ${p.rating}

<p>${p.review}</p>

<p class="price">Amazon ₹${p.online.amazon}</p>

<a href="${p.amazon}" target="_blank">Buy Amazon</a>

<p class="price">Flipkart ₹${p.online.flipkart}</p>

<a href="${p.flipkart}" target="_blank">Buy Flipkart</a>

<h4>Local Shops</h4>

${p.shops.map(s=>`
<div class="shop">
${s.name} - ₹${s.price}
<a href="${s.map}" target="_blank">Map</a>
</div>
`).join("")}

</div>

`

})

document.getElementById("productList").innerHTML=html

}

/* SEARCH */

function searchProduct(){

let q=document.getElementById("searchInput").value.toLowerCase()

let result=products.filter(p=>JSON.stringify(p).toLowerCase().includes(q))

displayProducts(result)

showFilters(q)

}

/* FILTERS */

function showFilters(query){

let html=""

if(query.includes("phone")||query.includes("mobile")){

html=`
<h3>Mobile Brands</h3>

<button onclick="filterBrand('Apple')">Apple</button>
<button onclick="filterBrand('Samsung')">Samsung</button>
<button onclick="filterBrand('OnePlus')">OnePlus</button>
`
}

else if(query.includes("face")||query.includes("skin")){

html=`
<h3>Skin Type</h3>

<button onclick="filterSkin('Oily')">Oily</button>
<button onclick="filterSkin('Dry')">Dry</button>
`
}

else if(query.includes("hair")){

html=`
<h3>Hair Type</h3>

<button onclick="filterHair('Normal')">Normal</button>
<button onclick="filterHair('Curly')">Curly</button>
`
}

document.getElementById("dynamicFilters").innerHTML=html

}

/* FILTER FUNCTIONS */

function filterBrand(b){
displayProducts(products.filter(p=>p.brand==b))
}

function filterSkin(t){
displayProducts(products.filter(p=>p.skin==t))
}

function filterHair(t){
displayProducts(products.filter(p=>p.hair==t))
}

/* ADMIN ADD PRODUCT */

function addProduct(){

let p={
name:document.getElementById("pname").value,
type:document.getElementById("ptype").value,
brand:document.getElementById("pbrand").value,
image:document.getElementById("pimage").value,
rating:"4.5",
review:"New product",
amazon:"#",
flipkart:"#",
online:{
amazon:document.getElementById("pamz").value,
flipkart:document.getElementById("pflp").value
},
shops:[{
name:document.getElementById("shopname").value,
price:document.getElementById("shopprice").value,
map:"https://maps.google.com"
}]
}

products.push(p)

alert("Product Added")

}

</script>

</body>
</html>
