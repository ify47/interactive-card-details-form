<script setup>
import Desktop from '../assets/bg-main-desktop.png';
import bgcardback from '../assets/bg-card-back.png';
import bgcardfront from '../assets/bg-card-front.png';
import cardIcon from '../assets/card-logo.svg';
import { computed, inject } from 'vue';

const bgDesktop = Desktop
const cardBack = bgcardback
const cardFront = bgcardfront
const icon = cardIcon

// const cardNumbersProps = inject('c_numbers' )
const { cardName, cardNumbers, cardMonth, cardYear, cardCvv } = inject('inputDetails')

const formattedCardNumbers = computed(() => {
    // Format the numbers with spaces after every 4 characters to be displayed
    return cardNumbers.value.replace(/(\w{4})(?=\w)/g, '$1 ');
})

</script>


<template>
    <div class="images">
        <div class="bg">
            <img class="bgimg" :src=bgDesktop alt="Background Image" />
        </div>
        <div class="cards">
            <div>
                <img class="logo" :src="icon" alt="card logo" />
                <h2 class="numbers">{{ formattedCardNumbers }}</h2>
                <div class="nameAndyy">
                    <div class="nameAndyyRw">
                        <p>{{ cardName }}</p>
                        <p class="mmyy">{{ cardMonth }}/{{ cardYear }}</p>
                    </div>
                </div>
                <img class="cardfront" :src=cardFront alt="Card Front" />
            </div>
            <div>
                <p class="cardcvv">{{ cardCvv }}</p>
                <img class="cardback" :src=cardBack alt="Card Back" />
            </div>
        </div>
    </div>
</template>


<style lang="scss" scoped>
.images {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;

    .bgimg {
        width: 31vw;
        height: 100vh;
    }

    .cards {
        display: flex;
        flex-direction: column;
        align-items: center;

        .cardfront {
            max-width: 24rem;
            margin: 0 0 30px -320px;
        }

        .cardback {
            max-width: 24rem;
            margin-left: -157px;
        }

        .logo {
            position: absolute;
            margin: 24px 0 0 -293px;
        }

        .numbers {
            position: absolute;
            margin: 128px 0 0 -294px;
            letter-spacing: 3px;
            font-weight: 500;
            color: white;
        }

        .nameAndyy {
            position: absolute;
            margin: 176px 0 0 -4px;


            .nameAndyyRw {
                display: flex;
                justify-content: space-between;
                margin-left: -291px;
                text-transform: uppercase;
                font-size: 11px;
                letter-spacing: 2px;
                color: white;

                .mmyy {
                    margin-right: -2.5rem;
                }
            }
        }

        .cardcvv {
            position: absolute;
            margin: 6rem 0 0 9.5rem;
            color: white;
            letter-spacing: 1px;
            font-size: 12px;
        }
    }
}

@media (max-width: 750px) {

    .images .cards .logo {
        margin: 18px 0 0 -168px;
        width: 4rem;
    }

    .images .cards .cardfront {
        max-width: 18rem;
        margin: 0 0 30px -191px;
    }

    .images .cards .cardback {
        max-width: 18rem;
        margin-left: -155px
    }

    .images .cards .numbers {
        margin: 94px 0 0 -180px;
        font-size: 17px;
    }

    .images .cards .nameAndyy .nameAndyyRw {
        margin-left: -179px;
        margin-top: -47px;
        font-size: 10px;

        .mmyy {
            margin-right: -4.5rem;
        }
    }

    .images .cards .cardcvv {
        margin: 4.4rem 0 0 4rem;
        font-size: 10px
    }
}

@media (max-width: 568px) {
    .images {
        flex-direction: column;
        align-items: unset;
        justify-content: unset;
        width: 100%;

        .bgimg {
            width: 100%;
            height: 200px;
            object-fit: cover;
        }

        .cards {
            position: relative;


            .logo {
                margin: -85px 0 0 -50px;
                width: 3rem;
                z-index: 1;
            }

            .numbers {
                margin: -30px 0 0 -49px;
                font-size: 13px;
                z-index: 1;
            }

            .nameAndyy {
                margin: 46px 0 0 107px;
                z-index: 1;

                .nameAndyyRw {
                    margin-left: -158px;
                    margin-top: -42px;
                    font-size: 9px;
                    letter-spacing: 1px;

                    .mmyy {
                        margin-right: -3.2rem;
                    }
                }
            }

            .cardfront {
                max-width: 15rem;
                margin: -100px 0 -3px -65px;
                position: relative;
            }

            .cardcvv {
                margin: -9.3rem 0 0 15rem;
            }

            .cardback {
                max-width: 15rem;
                margin-left: 58px;
                margin-top: -373px;
                margin-bottom: 92px;
            }
        }
    }
}
</style>