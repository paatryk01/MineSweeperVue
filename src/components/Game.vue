<template>
    <div id="game">
        <!-- <button class="newGame">New game</button> -->
        <h3 id='msg'>Good Luck</h3>
        <p>Bombs: {{ bombAmount }}</p>
        <div class="grid"></div>
    </div>
</template>

<script>

export default {

    name: 'Game',

    data() {
        return {
            width: 10,
            bombAmount: 20,
            flags: 0,
            squares: [],
            isGameOver: false
        }
    },

    methods: {

        prepareNewGame() {

            const grid = document.querySelector('.grid');

            const bombsArray = Array(this.bombAmount).fill('bomb');
            const emptyArray = Array(this.width * this.width - this.bombAmount).fill('empty');
            const gameArray = emptyArray.concat(bombsArray);
            const shuffledArray = gameArray.sort(() => Math.random() - 0.5);
            
                
            for(let i = 0; i < this.width * this.width; i++) {

                const square = document.createElement('div');
                square.setAttribute('id', i);
                square.classList.add(shuffledArray[i]);
                grid.appendChild(square);
                this.squares.push(square);
                let squaresArr = this.squares;
                let msg = document.querySelector('#msg');

                square.addEventListener('click', function() {

                    let currentId = square.id;
                    if(this.isGameOver) return;
                    if(square.classList.contains('checked') || square.classList.contains('flag')) return;
                    if(square.classList.contains('bomb')) {
                        
                        // this.gameOver();
                        
                        this.isGameOver = true;
                        msg.innerText = 'GAME OVER';

                        squaresArr.forEach((square) => {
                            
                            if(square.classList.contains('bomb')) {
                                square.innerHTML = '💣';
                            }
                        });

                    } else {
                        
                        let total = square.getAttribute('data');

                        if(total != 0) {

                            square.classList.add('checked');
                            square.innerHTML = total;
                            return;

                        }
                    }

                    square.classList.add('checked');
                })

                square.oncontextmenu = function(e) {
                    
                    e.preventDefault();
                    
                    if(this.isGameOver) return;
                    if(!square.classList.contains('checked') && (this.flags < this.bombAmount)) {

                        if(!square.classList.contains('flag')) {
                            
                            square.classList.add('flag');
                            square.innerHTML = '🚩';
                            this.flags++;
                        } else {

                            square.classList.remove('flag');
                            square.innerHTML = '';
                            this.flags--;
                        }
                    }
                }
            }
        },

        addNumbers() {

            for(let i = 0; i < this.squares.length; i++) {
                
                let total = 0;
                const isLeftEdge = (i % this.width === 0);
                const isRightEdge = (i % this.width === this.width - 1);

                if(this.squares[i].classList.contains('empty')) {

                    if(i > 0 && !isLeftEdge && this.squares[i - 1].classList.contains('bomb')) total++;
                    if(i > 9 && !isRightEdge && this.squares[i + 1 - this.width].classList.contains('bomb')) total ++;
                    if(i > 9 && this.squares[i - this.width].classList.contains('bomb')) total++;
                    if(i > 11 && !isLeftEdge && this.squares[i - 1 - this.width].classList.contains('bomb')) total ++;
                    if(i < 99 && !isRightEdge && this.squares[i + 1].classList.contains('bomb')) total ++;
                    if(i < 90 && !isLeftEdge && this.squares[i - 1 + this.width].classList.contains('bomb')) total ++;
                    if(i < 88 && !isRightEdge && this.squares[i + 1 + this.width].classList.contains('bomb')) total ++;
                    if(i < 90 && this.squares[i + this.width].classList.contains('bomb')) total++;

                    this.squares[i].setAttribute('data', total);
                }
            }
        },

        addFlag(square) {

            if(this.isGameOver) return;
            if(!square.classList.contains('checked') && (this.flags < this.bombAmount)) {

                if(!square.classList.contains('flag')) {

                    square.classList.add('flag');
                    square.innerHTML = '🚩';
                    this.flags++;
                } else {

                    square.classList.remove('flag');
                    square.innerHTML = '';
                    this.flags--;
                }
            }

        },

        checkSquare(square, currentId) {

            const isLeftEdge = (currentId % width === 0);
            const isRightEdge = (currentId % width === width - 1);

            setTimeout(() => {
                
            }, 10);

        }

        // gameOver() {
            
        //     this.isGameOver = true;

        //     this.squares.forEach((square) => {
                
        //         if(square.classList.contains('bomb')) {
        //             square.innerHTML = '💣';
        //         }
        //     })
        // }
    },

    mounted() {

        this.prepareNewGame();
        this.addNumbers();

    }

}

</script>

<style>
 
    .grid {
        height: 400px;
        width: 400px;
        display: flex;
        flex-wrap: wrap;
        background-color:rgb(235, 230, 223);
    }

    .grid div {
        height: 40px;
        width: 40px;
        border: 1px solid rgb(173, 169, 169);
        box-sizing: border-box;
    }

    .bomb {
        background-color: red;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .checked {
        background-color: green;
        color: black;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .flag {
        display: flex;
        align-items: center;
        justify-content: center;
    }

</style>