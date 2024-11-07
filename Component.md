<h1 align="center"> Components </h1>


# Local Storage

```js

const getAddedToCart = ()=> {
    const all = localStorage.getItem('addToCart')
    if(all){
        const addToCart = JSON.parse(all)
        
        return addToCart

    }else{
        return []
    }
}

const addCart = gadget => {
  
    const addToCart = getAddedToCart()
    const isExist = addToCart.find(item => item.id == gadget.id)
    if(isExist) return toast.error('This item already Exist! in Cart');

    addToCart.push(gadget)
    localStorage.setItem('addToCart', JSON.stringify(addToCart) )
    toast.success('Item Added Successfully!');
}

export {addCart , getAddedToCart }

```



# Use local storage in component

```js

    // use Component Data state
    const [gadget, setGadget] = useState({})

    // Add handler 
    const handleAddToCart = (gadget) => {
        addCart(gadget)
    }

    // Handle Button 
    <Button  onClick={()=> handleAddToCart(gadget)} className="btn"> Add to Cart  </Button>


```

<hr/>



#Tab Button Toggle (Ex, Cart - Wishlist)

main.jsx

```js
{
    path: '/dashboard',
    element: <Dashboard></Dashboard>,
    children:[
        {
        index: true, 
        element: <CartContent></CartContent>,
        },
        {
        path: 'cart',
        element: <CartContent></CartContent> ,
        },
        {
        path: 'wishlist',
        element: <WishContent></WishContent>,
        }
    ]
},

```

Button in component (Ex: Dashbord.jsx)

```js
// Outlet for Child COmponent, Use Location for Location path, useNavigate for Redirect Page
import { Outlet, useLocation, useNavigate } from "react-router-dom"; 

import CartContent from "./CartContent"; // import component (Ex: CartContent)
import { useState } from "react"; // USe for state Diclaration
import { Helmet } from "react-helmet-async"; // Use for dyamic browser Titel


const Dashboard = () => {

    const navigate = useNavigate();
    const location = useLocation()

    const [activeTab, setActiveTab] = useState("cart"); // USe for Button activetion 

    // When click Toggle button then Redirect another component (Ex: cart)
    const showCartContent = ()=> {
        setActiveTab("cart")
        navigate('/dashboard/cart')
    }

    // When click Toggle button then Redirect another component (Ex: Wishlist)
    const showWishContent = () => {
        setActiveTab("wishlist")
        navigate('/dashboard/wishlist')
    }
    

    return (
        <div>
            <Helmet>
                <title>Dashboard | Gadget Heaven</title>
            </Helmet>

            <div className=" bg-[#9538E2] pb-10 text-center pt-10">
                <h2 className="text-3xl font-bold text-white ">Dashboard</h2>
                <p className=" text-white mt-5 px-10 md:px-36 lg:px-72">explore the latest gadgets that will take your experience to the next level. from smart device to coolest accessories , we have it all.</p>

                <div className="container mx-auto">
                    <div role="tablist" className=" flex justify-center
                 tabs tabs-boxed  bg-transparent mt-10 gap-5 ">

                        // Cart Button with conditional active or deactive functionality.
                        <a onClick={showCartContent} role="tab" className={`tab !rounded-full w-40 font-bold ${activeTab === "cart" ? "bg-white text-black" : "border text-white"}`}>Cart</a>
                        

                        // Wishlish button with conditional active or deactive functionality.
                        <a onClick={showWishContent} role="tab" className={`tab !rounded-full w-40 font-bold ${activeTab === "wishlist" ? "bg-white text-black" : "border text-white"}`}>Wish List</a>
                    </div>
                </div>

            </div>
            
            // This is for display Default children Component in Parent component.
            {location.pathname === '/dashboard' ? <CartContent></CartContent> : <Outlet />}
            

            
        </div>
    );
};

export default Dashboard;

```
