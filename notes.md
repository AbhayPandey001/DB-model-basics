## Data storage
1. Sabse pehle yeh dekhte hain ki kya data store ho raha hai ,   
store to har baar data hi hoga but kaun se data points store karne hain woh pehle dekha jata hai .  
Data kaha store hoga uske bhi pehle yeh pata hona jaruri hai ki kya data rakhna hai.
for eg:: username , password , email etc.  
Iske liye bht sare tools hain unhi mese ek hai **mongoose**.. 
___
2. **Data modelling** : The structure of data (arrangement etc). 
for eg : user ke liye kaun si fields chahiye , doctor ke liye kaun si etc. and also yeh fields aapas mein kaise connect honge.   
paid tools for these are : **Moon modeler** (https://www.datensen.com/moon-modeler-for-databases.html) and eraserio(https://www.eraser.io/) [eraser ka free versionn bhi accha hai]
___
## simple method :
Take pen and paper (eraser.io ya kahi v , copy v chalegi) , usme screen banao .
    jo screen data save karne wali screen hai woh pehle banao

eg: Register page se data leke save karna hota hai whereas login page simple sa validation provide karata hai database se match karke ki koi user exist karta hai ya nahin. 
so  
    REGISTER > LOGIN (pehle register wali screen banao)

___
NOW , is wale part mein hamne online hi code kiya hai .  
kch aisi sites hain jo installation ke bina hi environments provide karti hain :
1. codesandbox (https://codesandbox.io/)
2. github codespace (https://github.com/features/codespaces)
3. Stackblitz(https://stackblitz.com/)

___
# Mongoose
ek schema bana ke rakh lete hain taki jaise hi db connect ho ye sab pehle se hi structure ban jaye : 
  
sabse pehle 
```
npm i mongoose
```
ab ham data models banayege files ka naam generally filename.models.js types me rakhte hain

yeh code same rehta hai har file mein almost:
```
import mongoose from "mongoose"

const userSchema = new mongoose.Schema({}) <--- is object mein schema define karte hain 

export const User = mongoose.model('User' , userSchema)

```
yeh structure har jagah same rehta hai 
## hota kya hai
export const user isliye kiye qki yeh wala schema kai aur jagah v use ho sakta hai waha import kr lenge.   
schema ke andr jo v define kiye hain uski help se ek schema ban jata hai joh directly database mein apply ho jata hai.  
ab directly usme values aati hai future mein and define nahi karna padta hai.  
data kaise rakhna hai woh schema object mein define hota hai.
___
mongoose.model do arguments leta hai , ek toh naam of model isi naam se schema banta hai halka edit hoke :    
**rule** : jo v name diye hain woh plural hojayega and lowercase completely so User -> users and Todo -> todos  
second argument leta hai ki kis basis pe model banana hai .   
Toh hamne uper jo schema banaya hota hai woh pass kar dete hain.
___

schema ke andr directly {
    username : String , 
    isactive : Boolean
}  
likh sakte hain 
lekin mongoose ek better facility provide karta hai production / professional level approach :
ek object define kar sakte hain fields ka v and unki property likh sakte hain aisa karne se backend logic ka lode kam hojata hai
```
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
    username : {
        type : String,    //type is a compulsary field when defining like this
        required : true ,
        unique : true , 
        lowercase : true //all values lowercase honi chahiye
    } , // itna karne se ek field ban gayi username ki 
    email : {
        type : String,
        required : true ,
        unique : true , 
        lowercase : true 
    } ,
    password : {
        type : String,
        required : true ,
        
    }
})

export const User = mongoose.model('User' , userSchema)
```
**type is compulsary to give**

kai baar time ko v track karna padta hai uske liye createdat and updated at already mil jate hain bas schema ke object mein second argument mein {timestamps : true}likh dena hai
```
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema(
    {-- regular code jo hai --} ,     ---->> comma lagana jaruri hai
    {timestamps : true}               ---->> timestamps hai timestamp nahi last mein s lagana hai as do chizein milti hain createdat & updatedat
)

export const User = mongoose.model('User' , userSchema)
```


there might be a need to relate two models (like users and todo)
uske liye schema mein : 
abc : {

}

do values leta hai ek type and ek refrence 

syntax : 
```
createdBy : {
    type : mongoose.Schema.Types.ObjectId , -----> iska mtlb mongoose ko ham bata rahe ki ham ab ek refrence dene wale hain jo relate hoga yaha pe
    ref : "Users"                           -----> wahi naam jo us wale model ke mongoose.model ke name argument mein paas kiya tha
}
```

We can also make another simple schema in the same file and use it in type of another schema in the main schema defining in the same file.  
Also enum can also be given as an attribute within an object in the property