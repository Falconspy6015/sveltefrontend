<script>
    import { onMount } from "svelte";
	

    let vehicleType = "two-wheeler";
    let color = "red";
    let battery = "standard";
    let cart = [];

    function addToCart() {
        cart = [...cart, { vehicleType, color, battery }];
    }

    function removeFromCart(index) {
        cart = cart.filter((_, i) => i !== index);
    }

    async function submitCart() {
        try {
            const response = await fetch("https://sveltebackend-i7f3.onrender.com/submit-cart", {

                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(cart),
            });

            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }

            const result = await response.json();
            alert(result.message);
			 cart = []; 
        } catch (error) {
            alert(`Error occurred while submitting the cart: ${error.message}`);
        }
    }
</script>

<header>
    <div class="navbar">
	    
        <img src="/logo.png" alt="Company Logo" class="logo" />
		 <h2>Customize Your EV</h2>
        <nav>
            <a href="#home">Home</a>
            <a href="#features">Features</a>
            <a href="#contact">Contact</a>
        </nav>
    </div>
</header>

<main class="container">
    <section class="customization">
        <h2>Customize Your EV</h2>
        <form>
            <div>
                <label for="vehicle-type">Choose Vehicle Type:</label>
                <select id="vehicle-type" bind:value={vehicleType}>
                    <option value="two-wheeler">Two-Wheeler</option>
                    <option value="three-wheeler">Three-Wheeler</option>
                    <option value="four-wheeler">Four-Wheeler</option>
                </select>
            </div>
            <div>
                <label for="color">Choose Color:</label>
                <select id="color" bind:value={color}>
                    <option value="red">Red</option>
                    <option value="blue">Blue</option>
                    <option value="green">Green</option>
                </select>
            </div>
            <div>
                <label for="battery">Choose Battery:</label>
                <select id="battery" bind:value={battery}>
                    <option value="standard">Standard</option>
                    <option value="long-range">Long-Range</option>
                </select>
            </div>
            <button type="button" on:click={addToCart} aria-label="Add to cart">Add to Cart</button>

        </form>
    </section>

    <section class="preview">
        <h3>Your Custom EV:</h3>
        <p>Type: {vehicleType}</p>
        <p>Color: {color}</p>
        <p>Battery: {battery}</p>
		<img src={`/vehicle.png`} alt="Vehicle Preview" />

    </section>

    <section class="cart">
        <h3>Cart</h3>
        {#if cart.length > 0}
            <ul>
                {#each cart as item, index}
                    <li>
                        <p>Type: {item.vehicleType}</p>
                        <p>Color: {item.color}</p>
                        <p>Battery: {item.battery}</p>
                        <button on:click={() => removeFromCart(index)}>Remove</button>
                    </li>
                {/each}
            </ul>
            <p>Total Items in Cart: {cart.length}</p>
            <button on:click={submitCart}>Submit Cart</button>
        {:else}
            <p>Your cart is empty.</p>
        {/if}
    </section>
</main>

<footer>
    <p>&copy; 2024 EV Company. All rights reserved.</p>
</footer>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
    font-family: 'Poppins', Arial, sans-serif;
    font-weight: 400; /* Regular weight for body text */
}

h2, h3 {
    font-weight: 700; /* Bold for headers */
    font-size: 2rem; /* Larger size for headings */
}

h1 {
    font-size: 2.5rem; /* Larger main title */
    font-weight: 900; /* Extra bold */
}

p {
    font-size: 1.1rem; /* Slightly larger paragraph text */
}


    .navbar {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        background-color: #000; /* Black background */
        border-bottom: 2px solid #444; /* Dark gray border */
    }

    .navbar a {
        color: #fff; /* White text */
        text-decoration: none;
        margin: 0 1rem;
        font-weight: bold;
        transition: color 0.3s;
    }

    .navbar a:hover {
        color: #FFD700; /* Gold hover effect */
    }

    .logo {
        height: 50px;
    }

    .container {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        gap: 2rem;
        padding: 2rem;
    }

    section {
        padding: 1.5rem;
        background: #000; /* Dark gray background */
		color: #fff; /* White text */
        border-radius: 10px;
        box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.8); /* Strong shadow */
    

    transition: transform 0.3s ease-out, opacity 0.3s ease;
    
    transform: translateY(20px);

	}
section:hover {
    transform: scale(1.01);
    /* Ensure opacity isn't unintentionally set to 0 */
}


    section h2, section h3 {
        margin-bottom: 1.5rem;
        color: #FFD700; /* Gold headers */
    }

    form div {
        margin-bottom: 1.5rem;
    }

    button {
        border: none;
        border-radius: 20px;
        padding: 0.75rem 1.5rem;
        background: #FFD700; /* Gold background */
        color: #000; /* Black text */
        font-weight: bold;
        cursor: pointer;
        transition: transform 0.2s, background 0.3s;
    }

    button:hover {
        transform: scale(1.05);
        background: #FFF; /* White hover effect */
        color: #000; /* Black text on hover */
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    ul li {
        padding: 1rem;
        background-color:#333; /* Medium gray background */
        border: 1px solid #444; /* Dark gray border */
        margin-bottom: 1rem;
        border-radius: 10px;
        color: #fff; /* White text */
    }

    ul li button {
        background-color: #e74c3c; /* Red for remove button */
        color: white;
        padding: 0.75rem 1.5rem;
        border: none;
        cursor: pointer;
        border-radius: 10px;
    }

    ul li button:hover {
        background-color: #c0392b;
    }

    .cart button {
        margin-top: 1.5rem;
    }

    footer {
        background-color: #000; /* Black background */
        color: #fff; /* White text */
        text-align: center;
        padding: 2rem 0;
        margin-top: 2rem;
        border-top: 2px solid #444; /* Dark gray border */
    }
	/* Style the dropdown (select elements) */
/* Style the dropdown (select elements) */
select {
    width: 100%; /* Ensure all selects are the same width */
    padding: 1rem 1.5rem; /* Increased padding for more space around the text */
    border: 2px solid #FFD700 !important; /* Gold border */
    border-radius: 25px !important; /* Rounded corners */
    background-color: #333 !important; /* Dark background color for the select */
    color: #fff !important; /* White text */
    font-size: 1rem;
    font-weight: 500;
    appearance: none !important; /* Remove default dropdown arrow */
    cursor: pointer;
    transition: all 0.3s ease;
    margin-bottom: 1.5rem; /* More spacing between dropdowns */
}

/* Remove the default select dropdown arrow */
select::-ms-expand {
    display: none !important; /* Remove default arrow for Internet Explorer */
}

/* When the select element is focused, apply a gold outline and glow effect */
select:focus {
    border-color: #FFD700 !important; /* Gold focus border */
    outline: none; /* Remove default outline */
    box-shadow: 0 0 5px rgba(255, 215, 0, 0.8); /* Gold glow effect */
}

/* Style the select dropdown options when clicked (to fit theme) */
select option {
    background-color: #333 !important; /* Dark background for options */
    color: #fff !important; /* White text for options */
    border-radius: 25px !important; /* Round edges for the dropdown options */
    padding: 1rem; /* More space around the text */
    transition: background-color 0.3s ease, border-radius 0.3s ease; /* Smooth transition for hover effects */
}

/* When hovering over an option, add a gold background */
select option:hover {
    background-color: #FFD700 !important; /* Gold background on hover */
    border-radius: 25px !important; /* Rounded edges on hover */
}

/* Additional space between form elements */
form div {
    margin-bottom: 2rem; /* Increased space between form groups */
}
/* Add margin to the top of the form */
form {
    margin-top: 2rem; /* Adds space between the form and the content above it */
}

/* Style the form labels with some space below them */
form label {
    display: block; /* Ensures the label is on its own line */
    margin-bottom: 0.75rem; /* Adds space between the label and the dropdown */
}
/* Mobile responsiveness adjustments */
@media (max-width: 768px) {
    /* Navbar adjustment */
    .navbar {
        flex-direction: column;
        align-items: flex-start;
    }

    /* Container adjustment */
    .container {
        grid-template-columns: 1fr; /* Single column for smaller screens */
        padding: 1rem; /* Reduce padding */
    }

    /* Customization, preview, and cart sections */
    .customization, .preview, .cart {
        padding: 1rem;
    }

    /* Buttons take full width on smaller screens */
    button {
        width: 100%;
		padding: 1rem 2rem; /* Larger touch targets */
    font-size: 1.2rem; /* Increased font size */
    }

    /* Adjust select dropdowns */
    select {
        width: 100%; /* Full width for dropdowns */
		font-size: 1.1rem; /* Larger font size for dropdowns */
    }

    /* Adjust the cart display */
    .cart ul {
        padding-left: 0;
    }
}
img {
    position: relative;  /* Ensure it can be layered */
    z-index: 1000;  /* Set a higher value to bring it to the front */
}
.preview img {
    width: 100%;  /* Make image take the full width of its parent */
    max-width: 600px;  /* Limit the maximum size to prevent overflow */
    height: auto;  /* Maintain aspect ratio */
    display: block;  /* Remove extra space below the image */
    margin: 0 auto;  /* Center the image horizontally */
}

</style>

