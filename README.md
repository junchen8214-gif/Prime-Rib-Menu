<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>David’s Menu Manual</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 font-sans text-gray-900">

    <header class="bg-white border-b sticky top-0 z-10 shadow-sm">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
            <h1 class="text-2xl font-bold text-orange-600">David’s Menu Manual</h1>
            <input type="text" id="searchInput" placeholder="Search ingredients or dishes..." 
                   class="border rounded-lg px-4 py-2 w-full md:w-1/3 focus:outline-none focus:ring-2 focus:ring-orange-500">
        </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 py-8 flex flex-col md:flex-row gap-8">
        <aside class="w-full md:w-1/4">
            <div class="sticky top-24 space-y-4">
                <h2 class="font-bold text-gray-500 uppercase text-xs tracking-widest">Categories</h2>
                <nav id="categoryFilters" class="flex flex-row md:flex-col gap-2 overflow-x-auto pb-4 md:pb-0">
                    <button class="whitespace-nowrap px-4 py-2 rounded-lg bg-orange-600 text-white font-semibold">All Items</button>
                    </nav>
            </div>
        </aside>

        <section id="menuGrid" class="w-full md:w-3/4 grid grid-cols-1 md:grid-cols-2 gap-6">
            <p class="text-gray-500 italic">Loading delicious food...</p>
        </section>
    </main>

    <script>
        // This is where the magic happens
        async function loadMenu() {
            try {
                // 1. Fetch the data from your JSON file
                const response = await fetch('menu.json');
                const menuItems = await response.json();
                
                const menuGrid = document.getElementById('menuGrid');
                menuGrid.innerHTML = ''; // Clear the "Loading" text

                // 2. Loop through each item and create the HTML
                menuItems.forEach(item => {
                    const card = document.createElement('div');
                    card.className = "bg-white rounded-xl shadow-sm border overflow-hidden hover:shadow-md transition duration-300";
                    
                    // Generate Allergen Tags
                    const allergenHTML = item.allergens.map(a => 
                        `<span class="text-[10px] bg-red-50 text-red-600 px-2 py-1 rounded border border-red-100 font-bold uppercase">${a}</span>`
                    ).join('');

                    card.innerHTML = `
                        <div class="h-48 bg-gray-100 flex items-center justify-center text-gray-400">
                            <span class="text-xs">Image for ${item.name}</span>
                        </div>
                        <div class="p-5">
                            <div class="flex justify-between items-start mb-2">
                                <h3 class="text-lg font-bold text-gray-800">${item.name}</h3>
                                <span class="text-green-600 font-bold">$${item.price}</span>
                            </div>
                            <p class="text-sm text-gray-600 mb-4">${item.description}</p>
                            <div class="mb-4 flex flex-wrap gap-2">${allergenHTML}</div>
                            <div class="bg-orange-50 p-3 rounded-lg border border-orange-100">
                                <p class="text-xs font-bold text-orange-800 uppercase mb-1">Staff Prep Note:</p>
                                <p class="text-xs text-orange-700 italic">${item.staff_note}</p>
                            </div>
                        </div>
                    `;
                    menuGrid.appendChild(card);
                });
            } catch (error) {
                console.error("Error loading menu:", error);
                document.getElementById('menuGrid').innerHTML = '<p class="text-red-500">Failed to load menu. Make sure menu.json is in the same folder!</p>';
            }
        }

        // Run the function when the page loads
        window.onload = loadMenu;
    </script>
</body>
</html>
