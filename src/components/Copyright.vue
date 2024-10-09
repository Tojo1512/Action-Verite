<template>
    <div class="copyright">
        <a href="https://www.facebook.com/profile.php?id=100067693985058&mibextid=LQQJ4d" target="_blank" rel="noopener noreferrer" class="copyright-link">
            <span>&copy; {{ currentYear }} Tojo Tianh</span>
            <img class="facebook-logo" :src="facebookLogoUrl" alt="Logo Facebook" />
        </a>
    </div>
</template>

<script>
export default {
    name: 'Copyright',
    data() {
        return {
            facebookLogoUrl: 'https://www.facebook.com/images/fb_icon_325x325.png',
            currentYear: new Date().getFullYear()
        }
    }
}
</script>

<style scoped>
.copyright {
    padding: 0.8rem;
    background-color: rgba(0, 0, 0, 0.05);
    box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
}

.copyright-link {
    display: flex;
    align-items: center;
    justify-content: center;
    color: #333;
    text-decoration: none;
    font-size: 0.9rem;
    transition: color 0.3s ease;
    color: white;
}

.copyright-link:hover {
    color: #1877f2;
}

.copyright-link span {
    margin-right: 0.5rem;
}

.facebook-logo {
    width: 20px;
    height: 20px;
    transition: transform 0.3s ease;
}

.copyright-link:hover .facebook-logo {
    transform: scale(1.1);
}

@media (max-width: 600px) {
    .copyright-link {
        flex-direction: column;
        font-size: 0.8rem;
    }

    .copyright-link span {
        margin-right: 0;
        margin-bottom: 0.3rem;
    }
}
</style>