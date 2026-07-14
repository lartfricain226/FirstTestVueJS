<template>
    <div class="layout">
        <header>
            <slot name="header"></slot>
        </header>
        <aside>
            <slot name="aside"></slot>
        </aside>
        <main>
            <slot name="main"></slot>
        </main>
        <footer>
            <slot name="footer"></slot>
        </footer>
    </div>
</template>

<style scoped>
.layout {
    display: grid;
    grid-template-columns: 200px 1fr;
}

.layout > * {
    border: 1px solid var(--pico-secondary-background);
    padding: 10px;
}

.layout header,
.layout footer {
    grid-column: 1 / -1;
}

header {
    background-color: green;
    color: white;
}
aside {
    background-color: gray;
}
footer  {
    text-align: center;
    background-color: green;
}
</style>