## 빙고 게임

- 작성자: 박인애 (inae park)
- 작성날짜: 2023-12-01
- 언어: Python


```python
import random

class Player:
    '''
    1. 게임 시작 전
    - 이름을 입력하여 플레이어 추가
    '''
    #플레이어 객체를 초기화
    def __init__(self, name):
        self.name = name
        self.bingo_cnt = 0
        self.table = []
    
    #객체를 문자열 표현(name)으로 반환하는 메서드
    def __str__(self) -> str:
        return f"{self.name}"


class Game:
    '''
    1. 게임 시작 전
    - 나를 제외한 플레이어 3명과 빙고게임 진행
    '''
    #게임 객체를 초기화
    def __init__(self):
        self.players = [] # 플레이어들의 목록
        self.my_player = None   # 사용자 이름(문제 1-(1)에서 입력받음)
        self.deck = [i for i in range(1, 10)] # 숫자 생성
        print(f'deck의 range: {range(len(self.deck))}')
        
        #초기 플레이어 3명 생성
        self.players.append(Player("박멍멍"))
        self.players.append(Player("김야옹"))
        self.players.append(Player("박인애"))
    
    '''
    1. 게임 시작 전
    - 4명의 플레이어의 순서를 랜덤으로 지정
    - 각 플레이어의 빙고판(1~9 사이의 정수, 중복 불가) 생성
    - 이름과 함께 빙고판 출력
    '''
    def start_game(self):
        """
        - [ 게임 시작 전 ] 초기 설정을 하고, 플레이어의 처음 빙고판을 출력하는 함수입니다.
        - 1-(1) 플레이어 이름을 입력받고 플레이 순서를 랜덤하게 섞어주세요
        - 1-(2) 랜덤한 숫자로 플레이어들의 빙고판을 세팅하고 출력합니다. 
        - 동일 클래스의 game()에서 호출됩니다.
        """
        print("=============================")
        print("      빙고게임 시작 ^_^      ")
        print("=============================")

        # 사용자로부터 플레이어의 이름을 입력받아 플레이어들의 목록에 추가하고, 순서를 랜덤하게 섞어주세요.
        # 사용자로부터 입력받기
        self.my_player = input("당신의 이름을 입력해주세요 (ex. 홍길동)")
        # 플레이어 객체 생성
        player = Player(name=self.my_player)
        # 플레이어 목록(리스트)에 추가
        self.players.append(player)
        # 플레이어 목록 순서 랜덤으로 섞기; 이 부분은 random.shuffle을 이용하면 됨
        random.shuffle(self.players)

        # 각각의 플레이어들의 빙고판을 생성한 후, 출력해 주세요.
        # (단, 각 플레이어들의 빙고판은 1부터 9까지의 숫자들이 중복없이 구성되어야 합니다.)
        for i in range(len(self.players)):
            # 플레이어 선택
            p = self.players[i]
            print(f'플레이어 이름 : {p.name}\n')
            print("=== 플레이어 테이블 ===")
            
            # 빙고판 생성
            #p.table = [[1, 1, 1], [1, 1, 1], [1, 1, 1]]
            
            # temp 배열은 각 플레이어 별로 새로 생성하는 배열로 랜덤 값의 중복 여부를 확인하기 위한 용도
            temp = []
            square_matrix_size = 3   # 빙고판: 3x3 matrix (2차원 배열(텐서))
            for j in range(square_matrix_size):  # 3행
                row = []
                for k in range(square_matrix_size): # 3열
                    number = random.randint(1, 9)  # 1 ~ 9 정수 중 랜덤수
                    
                    # 중복값이 아닐 때까지 다시 뽑기
                    while number in temp:
                        number = random.randint(1, 9)
                        
                    row.append(number)
                    temp.append(number)
                
                p.table.append(row)
                
                # 빙고판 출력
                print(row)

    
    '''
    2. 게임 진행
    - 플레이어 순서 안내
    - 3빙고가 된 플레이어가 생기면 종료
    '''
    def set_next_player(self, round_num):
        """ 
        - [ 게임 진행 ] round_num을 매개변수로 받아서 해당 라운드의 플레이어를 지정하는 함수 입니다. (round_num은 play_game에 정의되어 있습니다)
        - 2-(1) round_num이 플레이어들의 수보다 크더라도 에러가 나지 않도록 모든 플레이어들이 차례대로 지정될 수 있도록 구현하세요.
        - 2-(2) 해당 라운드에 지정된 플레이어 객체를 return해 주세요.
        - 동일 클래스의 play_game()에서 호출됩니다.
        """
        # 리스트의 값의 개수를 초과해서 반복해도 리스트의 길이를 벗어나지 않도록 해주세요.
        # 이 부분은 모듈러를 이용해 구현하면 된다
        # round는 1부터, players 리스트의 index는 0부터 시작하므로 round-1을 기준으로
        rn = (round_num-1) % 4
        
        # 다음 순서의 플레이어 객체를 return해 주세요.
        # 위에 start_game에서 플레이어 순서는 랜덤으로 재배정하였다
        # 또 랜덤으로 순서 재배치할 필요없이 현재 플레이어 리턴하면 될듯
        return self.players[rn]


    def count_bingo(self):
        """ 
        - [ 게임 진행 ] player의 빙고 개수를 업데이트 하는 함수입니다.
        - 각각의 플레이어들을 순회하면서 bingo_cnt를 카운트해주세요.
        - 플레이어의 빙고 개수를 초기화하고
        - 각각 빙고를 체크합니다.
        - 동일 클래스의 play_game()에서 호출됩니다.
        """
        for player in self.players:
            # 플레이어의 빙고 개수를 초기화합니다.
            player.bingo_cnt = 0
            
            # Case 1
            '''
            [x_0, y_0, z_0]
            [x_1, y_1, z_1]
            [x_2, y_2, z_2]
            '''
            # 플레이어의 빙고판 가로를 확인해 주세요
            for x, y, z in player.table:
                for i in range(3):
                    if x == 0 and y == 0 and z == 0:
                        player.bingo_cnt += 1  # 빙고!
            
            # Case 2
            '''
            [x_0, x_1, x_2]
            [y_0, y_1, y_2]
            [z_0, z_1, z_2]
            '''
            # 플레이어의 빙고판 세로를 확인해 주세요
            transpose = [list(i) for i in zip(*player.table)]
            for x, y, z in transpose:
                for i in range(3):
                    if x == 0 and y == 0 and z == 0:
                        player.bingo_cnt += 1  # 빙고!

            
            # Case 3
            '''
            [00, - , - ]
            [- , 11, - ]
            [- , - , 22]
            '''
            # 플레이어의 빙고판 대각선(좌상단 -> 우하단) 확인
            cnt = 0
            for i in range(3):
                if player.table[i][i] == 0:
                    cnt = cnt+1
                
            if cnt == 3:
                    player.bingo_cnt += 1  # 빙고!

            
            
            # Case 4
            '''
            2차원 배열은 음의 index 이용 (python식 거꾸로 순서)
            [- , - , 02]
            [- , 11, - ]
            [20 , - , -]
            '''
            # 플레이어의 빙고판 대각선(우상단 -> 좌하단) 확인
            cnt = 0
            for i in range(3):
                for j in range(3):
                    if player.table[i][-(j+1)] == 0:
                        player.bingo_cnt += 1  # 빙고!
                        
            if cnt == 3:
                player.bingo_cnt += 1


    def do_bingo(self, picked_num):
        """ 
        - [ 게임 진행 ] 빙고판에서 선택한 숫자를 매개변수로 받아서 0으로 바꾸는 메서드입니다. (picked_num은 play_game함수에 정의되어 있습니다.)
        - 플레이어들을 순회하면서 빙고!인 숫자를 숫자 0으로 바꿔주세요.
        - 동일 클래스의 play_game에서 호출됩니다.
        """ 
        # 플레이어들을 순회하면서 빙고!인 숫자를 숫자 0으로 바꿔주세요. 
        for p in self.players:
            for j in range(len(p.table)):
                for k in range(len(p.table[j])):
                    if p.table[j][k] == picked_num:
                        p.table[j][k] = 0


    def play_game(self):
        """
        - [ 게임 진행 ] 부분을 담당하는 함수 입니다.
        - 라운드를 시작하고 각 플레이어의 순서를 결정하여 게임을 진행합니다. 
        - 동일 클래스의 game()에서 호출됩니다.
        """
        print("=============================")
        print("          게임 순서          ")
        print("=============================")

        play_order = ", ".join(map(str, self.players))
        print(f"게임은 {play_order} 순으로 진행됩니다.\n")

        round_num = 0
        while True:
            round_num += 1

            print("=============================")
            print(f"       ROUND {round_num} - START")
            print("=============================")
            
            next_player = self.set_next_player(round_num)
            print(f"이번은 {next_player} 차례입니다. ^_^ \n")
            
            if next_player.name == self.my_player: 
                while True: #예외처리
                    try:
                        picked_num = int(input(f'당신의 차례입니다. 남은 숫자 중 하나를 입력해주세요. *남은 숫자*: {sorted(self.deck)}'))
                        if picked_num in self.deck:
                            break
                        elif picked_num > 9 or picked_num < 0:
                            print('숫자 범위를 벗어납니다. 1~9 사이의 숫자 중 뽑지 않은 숫자(남은 숫자)를 입력해주세요.')
                            continue
                        elif picked_num not in self.deck:
                            print('이미 뽑은 숫자를 입력하셨습니다. 남은 숫자 중 다시 입력해주세요.')
                    except ValueError:
                        print('정수가 아닌 숫자를 입력하셨습니다. 1~9 사이 숫자 중 뽑지 않은 숫자를 입력해주세요.')

                    except Exception as e:
                        print(e)
            #컴퓨터는 남은 숫자 중 랜덤으로 선택합니다.
            else:
                picked_num = random.choice(self.deck)

            #한번 뽑은 숫자를 또 뽑을 수 없기 때문에 뽑은 숫자를 deck에서 삭제해줍니다.
            self.deck.remove(picked_num)
            print(f'{next_player}(이)가 {picked_num}을 선택했습니다.')

            #do_bingo함수를 이용하여 선택한 숫자 빙고판에서 0으로 바꿉니다. 
            self.do_bingo(picked_num) 

            # 빙고 수를 세는 부분입니다.
            self.count_bingo()

            #빙고를 화면에 표시하는 부분입니다.
            for player in self.players:
                print(f'>> {player.name} (현재 빙고: {player.bingo_cnt})')
                for row in player.table:
                    print(row)
                print()

            print("=============================")
            print(f"       ROUND {round_num} - END")
            print("=============================")
            
            #플레이어 중 3빙고 이상 달성한 플레이어 있을 경우 함수가 종료됩니다.
            for player in self.players:
                if player.bingo_cnt >= 3: return
            print()
    
    
    def game_result(self):
        """
        - [게임 종료] 게임 결과를 출력하는 메서드입니다.
        - (1) 빙고 개수를 기준으로 내림차순으로 정렬하되, 만약 빙고 개수가 같다면 이름을 기준으로 오름차순으로 정렬해 주세요. (* 동점자 처리 주의!!)
        - (2) 사용자의 경우 이름 옆에 *을 붙여서 출력해주세요.(ex. *홍길동*)
        """
        print("=============================")
        print("     게임 순위 - 빙고 개수")
        print("=============================")
        
        # 빙고 개수를 기준으로 '내림차순' 정렬. (default 값)
        # 빙고 개수가 같다면 이름을 기준으로 오름차순 정렬 (빙고 개수와 이름이 모두 같은 경우는 고려하지 않습니다.)
        # lambda함수에 대해 공부해 보세요! 물론 lambda 함수를 사용하지 않고 구현해도 좋습니다. 
        sorted_players = self.players
        
        ## bingo cnt는 내림차순, 이름은 오름차순으로 정렬
        sorted_players = sorted(sorted_players, key=lambda player: (-player.bingo_cnt, player.name))
        
        
        
        # 사용자의 경우 이름 옆에 *을 붙여서 출력해주세요.(ex. *홍길동*)
        # 점수가 같으면 등수도 같도록 반드시 동점자 처리를 해주세요.
        
        # 1등부터 시작
        rank = 1
        
        # sorted_players에 대해 순차적으로 확인
        for i in range(len(sorted_players)):
            p = sorted_players[i]
            
            # 이미 sorting 했으니깐 등수 매기기 쉬움
            # 이전과 겹치지 않으면 (중복아니면) 등수 + 1
            if i > 0 and p.bingo_cnt != sorted_players[i-1].bingo_cnt:
                rank += 1
              
            # 등수 출력
            print(f'{rank}등 - ')
            
            # 이름 출력
            # (1) player가 사용자 정의 player면 이름 옆에 **를 붙이기
            if p.name == self.my_player:
                print(f'*{p.name}* : 빙고 {p.bingo_cnt}번')
            # (2) 그 외 player
            else:
                print(f'{p.name} : 빙고 {p.bingo_cnt}번')
    


    def game(self):
        """
        - 게임 운영을 위한 함수입니다.
        """
        self.start_game()
        self.play_game()
        self.game_result()


if __name__ == "__main__":  
    """
    - 코드를 실행하는 부분입니다.
    """
    game = Game()

    game.game()
```

    deck의 range: range(0, 9)
    =============================
          빙고게임 시작 ^_^      
    =============================
    당신의 이름을 입력해주세요 (ex. 홍길동)초코바나나
    플레이어 이름 : 김야옹
    
    === 플레이어 테이블 ===
    [4, 7, 8]
    [9, 1, 6]
    [3, 5, 2]
    플레이어 이름 : 초코바나나
    
    === 플레이어 테이블 ===
    [1, 7, 4]
    [2, 6, 3]
    [8, 5, 9]
    플레이어 이름 : 박멍멍
    
    === 플레이어 테이블 ===
    [5, 4, 7]
    [8, 1, 9]
    [6, 3, 2]
    플레이어 이름 : 박인애
    
    === 플레이어 테이블 ===
    [9, 6, 7]
    [5, 3, 8]
    [2, 4, 1]
    =============================
              게임 순서          
    =============================
    게임은 김야옹, 초코바나나, 박멍멍, 박인애 순으로 진행됩니다.
    
    =============================
           ROUND 1 - START
    =============================
    이번은 김야옹 차례입니다. ^_^ 
    
    김야옹(이)가 5을 선택했습니다.
    >> 김야옹 (현재 빙고: 1)
    [4, 7, 8]
    [9, 1, 6]
    [3, 0, 2]
    
    >> 초코바나나 (현재 빙고: 1)
    [1, 7, 4]
    [2, 6, 3]
    [8, 0, 9]
    
    >> 박멍멍 (현재 빙고: 1)
    [0, 4, 7]
    [8, 1, 9]
    [6, 3, 2]
    
    >> 박인애 (현재 빙고: 1)
    [9, 6, 7]
    [0, 3, 8]
    [2, 4, 1]
    
    =============================
           ROUND 1 - END
    =============================
    
    =============================
           ROUND 2 - START
    =============================
    이번은 초코바나나 차례입니다. ^_^ 
    
    당신의 차례입니다. 남은 숫자 중 하나를 입력해주세요. *남은 숫자*: [1, 2, 3, 4, 6, 7, 8, 9]5
    이미 뽑은 숫자를 입력하셨습니다. 남은 숫자 중 다시 입력해주세요.
    당신의 차례입니다. 남은 숫자 중 하나를 입력해주세요. *남은 숫자*: [1, 2, 3, 4, 6, 7, 8, 9]1
    초코바나나(이)가 1을 선택했습니다.
    >> 김야옹 (현재 빙고: 2)
    [4, 7, 8]
    [9, 0, 6]
    [3, 0, 2]
    
    >> 초코바나나 (현재 빙고: 2)
    [0, 7, 4]
    [2, 6, 3]
    [8, 0, 9]
    
    >> 박멍멍 (현재 빙고: 2)
    [0, 4, 7]
    [8, 0, 9]
    [6, 3, 2]
    
    >> 박인애 (현재 빙고: 2)
    [9, 6, 7]
    [0, 3, 8]
    [2, 4, 0]
    
    =============================
           ROUND 2 - END
    =============================
    
    =============================
           ROUND 3 - START
    =============================
    이번은 박멍멍 차례입니다. ^_^ 
    
    박멍멍(이)가 8을 선택했습니다.
    >> 김야옹 (현재 빙고: 3)
    [4, 7, 0]
    [9, 0, 6]
    [3, 0, 2]
    
    >> 초코바나나 (현재 빙고: 3)
    [0, 7, 4]
    [2, 6, 3]
    [0, 0, 9]
    
    >> 박멍멍 (현재 빙고: 3)
    [0, 4, 7]
    [0, 0, 9]
    [6, 3, 2]
    
    >> 박인애 (현재 빙고: 3)
    [9, 6, 7]
    [0, 3, 0]
    [2, 4, 0]
    
    =============================
           ROUND 3 - END
    =============================
    =============================
         게임 순위 - 빙고 개수
    =============================
    1등 - 
    김야옹 : 빙고 3번
    1등 - 
    박멍멍 : 빙고 3번
    1등 - 
    박인애 : 빙고 3번
    1등 - 
    *초코바나나* : 빙고 3번
    


```python

```
