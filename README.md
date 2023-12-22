
import random

class GameBoard:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.board = [['-' for _ in range(width)] for _ in range(height)]

    def display_board(self):
        for row in self.board:
            print(" ".join(row))

class Army:
    def __init__(self, name, position):
        self.name = name
        self.position = position
        self.strength = 3

class Game:
    def __init__(self, width, height):
        self.board = GameBoard(width, height)
        self.player_army = [Army('Player', (x, y)) for x in range(width // 2) for y in range(height) if x < width // 4]
        self.computer_army = [Army('Computer', (x, y)) for x in range(width - 1, width // 2 - 1, -1) for y in range(height) if x >= width // 4 * 3]

    def place_armies(self):
        for soldier in self.player_army:
            x, y = soldier.position
            self.board.board[y][x] = 'P'  # P لتمثيل الجيش الخاص باللاعب
        for soldier in self.computer_army:
            x, y = soldier.position
            self.board.board[y][x] = 'C'  # C لتمثيل الجيش الخاص بالحاسوب

    def check_winner(self):
        player_alive = sum(soldier.strength > 0 for soldier in self.player_army)
        computer_alive = sum(soldier.strength > 0 for soldier in self.computer_army)

        if player_alive == 0:
            return "Computer wins!"
        elif computer_alive == 0:
            return "Player wins!"
        else:
            return None

    def attack(self, attacker, target):
        if target.strength > 0:
            # هجوم بسيط باستخدام قوة الهجوم
            damage = random.randint(1, 3)
            target.strength -= damage
            print(f"{attacker.name}'s attack causes {damage} damage to {target.name}'s unit!")

    def play_game(self):
        print("Welcome to the game!")
        self.board.display_board()

        while True:
            self.place_armies()
            winner = self.check_winner()

            if winner:
                print(winner)
                break

            for player_soldier, computer_soldier in zip(self.player_army, self.computer_army):
                if player_soldier.strength > 0:
                    self.attack(player_soldier, computer_soldier)
                if computer_soldier.strength > 0:
                    self.attack(computer_soldier, player_soldier)

# لتشغيل اللعبة:
game = Game(10, 5)  # يمكنك تغيير الأبعاد هنا
game.play_game()
