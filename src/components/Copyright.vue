<template>
    <div class="copyright">
        <p>&copy; 2023 Tojo Tianh</p>
        <a href="https://www.facebook.com/tojo.tianh" target="_blank" rel="noopener noreferrer">
            <img src="/path/to/facebook-logo.png" alt="Facebook" class="facebook-logo">
        </a>
    </div>
</template>

<script setup lang="ts">
// Aucun script nécessaire pour le moment
</script>

<style scoped>
.copyright {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 1rem;
    background-color: rgba(0, 0, 0, 0.1);
}

.copyright p {
    margin-right: 1rem;
}

.facebook-logo {
    width: 24px;
    height: 24px;
}
</style>