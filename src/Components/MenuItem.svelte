<script>
    export let label;
    export let icon;
    const slugify = text => {
        return text.toString().toLowerCase()
                            .replace(/\s+/g, '-')
                            .replace(/[^\w\-]+/g, '')
                            .replace(/\-\-+/g, '-')
                            .replace(/^-+/, '')
                            .replace(/-+$/, '')
    }
</script>

<li>
    <a href="#{slugify(label)}" class="item item--{slugify(label)}">
        <img src="{icon}" alt="{label}" />
        <span>{label}</span>
    </a>
</li>

<style type="text/scss">
    @import "../style/theme.scss";
    li {
        float: left;
        padding: 14px 10px;
        a.item {
            color: $color_link;
            font-size: 14px;
            text-decoration: none;
            img {
                width: 25px;
                height: auto;
                padding-right: 5px;
            }
        }
    }

    @media (max-width: 1200px){
        li {
            a.item {
                text-align: center;
                display: block;
                img {
                    display: block;
                    margin: 0 auto;
                }
            }
        }
    }
</style>