<script>
    import { Col, Container, Row } from 'sveltestrap';
    import MenuItem from './MenuItem.svelte';
    import Hamburger from './Hamburger.svelte';

    let items = [
        {label: 'Scegli',           icon:'https://svgshare.com/i/MrS.svg'},
        {label: 'Domande frequenti',icon:'https://svgshare.com/i/MrE.svg'},
        {label: 'Tariffario',       icon:'https://svgshare.com/i/Mq4.svg'},
        {label: 'Contatti',         icon:'https://svgshare.com/i/Mqa.svg'},
        {label: 'Carrello',         icon:'https://svgshare.com/i/Mr3.svg'},
        {label: 'Login',            icon:'https://svgshare.com/i/MpK.svg'},
    ]
</script>

<header>
    <Container>
        <Row>
            <Col lg="3" sm="12" xs="12" md="3">
                <Hamburger className="d-block d-md-none"/>
                <a href="/" id="logo">
                    <img src="https://rent.decathlon.it/assets/rent-logo-decathlon.webp" alt="logo" class="img-fluid" />
                </a>
            </Col>
            <Col sm="6" xs="12" lg="9" md="9">
                <ul id="menu" class="d-none d-md-block">
                    {#each items as item}
                        <MenuItem label={item.label} icon={item.icon} />
                    {/each}
                </ul>
            </Col>
        </Row>
    </Container>
</header>

<style type="text/scss">
    @import "../style/theme.scss";
    header {
        padding: 10px 0;
        position: relative;
        #logo {
            img {
                margin-top: 5px;
                max-width: 262px;
                width: 100%;
                height: auto;
            }
        }
        ul#menu,
        ul#menu li {
            @extend .list-nostyle;
        }

        ul#menu {
            float: right;
        }
    }

    @media (max-width: 768px){
        #logo {
            position: absolute;
            left: 60px;
        }
    }
</style>