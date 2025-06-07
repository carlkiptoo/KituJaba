<template>
    <nav class="navbar">
        <div class="nav-container">
            <div class="logo">
                <img src="../assets/logo.png" alt="logo">

            </div>
        </div>
    </nav>
</template>

<style scoped>
.navbar {
    background: #cf9230;
    background: linear-gradient(90deg, rgba(207, 146, 48, 1) 0%, rgba(148, 86, 47, 1) 50%, rgba(108, 36, 32, 1) 100%);
    padding: 1rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.nav-container {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    justify-items: left;
}

.logo img {
    height: 40px;
    object-fit: contain;
}
</style>