<script>
    import { Col, Container, Row } from 'sveltestrap';
    import FindCity from "./FindCity.svelte";
</script>

<Container>
    <div>
        <Row>
            <Col xs="12" sm="10">
                <FindCity />
            </Col>
            <Col xs="12" sm="2">
                <button type="submit" class="btn btn-primary">
                    <span>CERCA</span>
                </button>
            </Col>
        </Row>
    </div>
</Container>

<style type="text/scss">
    @import "../style/theme.scss";
    div {
        position: relative;
        background: #fff;
        padding: 10px;
        bottom: 35px;
        box-shadow: 0 2px 10px rgba(0,0,0,.1);

        button[type="submit"]{
            width: 100%;
            height: 55px;
            border-radius: 0;
            border: 0 none;
            box-shadow: none;
            font-weight: 600;
            background-color: $color_btn-primary;
            position: relative;
            overflow: hidden;
            span {
                position: relative;
                z-index: 2;
            }
            &:before {
                content: '';
                position: absolute;
                z-index: 2;
                right: 0;
                top: 0;
                bottom: 0;
                width: 24px;
                background-image: url("https://svgshare.com/i/Mpe.svg");
                background-size: 15px 15px;
                background-repeat: no-repeat;
                background-position: right 8px center;
            }
            &:after {
                content: '';
                position: absolute;
                top: 100%;
                left: -25%;
                height: 150%;
                width: 150%;
                background-color: $color_btn-primary--hover;
                z-index: 1;
                transform-origin: calc(83.33333% - 3.33333em) 0;
                transition: transform .18s ease,-webkit-transform .18s ease,-moz-transform .18s ease;
                transform: rotate(-10deg);
            }
            &:hover {
                &:after {
                    transform: rotate(0) translateY(-100%);
                    transition: transform .36s ease,-webkit-transform .36s ease,-moz-transform .36s ease;
                }
            }
        }

    }
    @media (max-width: 768px){
        div {
            button[type="submit"]{ 
                margin-top: 10px;
            }
        }
    }
</style>

