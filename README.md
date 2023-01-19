# RockPaperScissors

I created a simple Rock, Paper, Scissors game but with a bit of a twist. Instead of trying to beat your opponent which is the computer, you are decided if you are the winner or if you are the loser.


import SwiftUI

struct ContentView: View {
    let moves = ["✊", "✋", "✌️"]
    
    @State private var computerChoice = Int.random(in: 0..<3)
    
    @State private var playerShouldWin = Bool.random()
    
    @State private var score = 0
    
    @State private var questionCount = 1
    
    @State private var showingResults = false
    
    
    var body: some View {
        ZStack {
            LinearGradient(gradient: Gradient(colors: [.pink, .mint]), startPoint: .top, endPoint: .bottom) //colors the enter background of the screen with gradient
                .ignoresSafeArea() //ensures color goes right to the edge of the screen
        VStack {
            
            Spacer()
            Text("Computer has played...")
                .font(.headline)
            Text(moves[computerChoice])
                .font(.system(size: 200))
            
            if playerShouldWin {
                Text("Which one wins?")
                    .foregroundColor(.green)
                    .font(.title)
            } else {
                Text("Which one loses?")
                    .foregroundColor(.indigo)
                    .font(.title)
            }
            
            
            HStack {
                ForEach(0..<3) { number in
                    Button(moves[number]) {
                        play(choice:number)
                    }
                    .font(.system(size: 80))
                }
            }
            
            Spacer()
            
            Text("Score: \(score)")
                .font(.subheadline)
            
            Spacer()
        }
    }
        .alert("Game over", isPresented: $showingResults) {
            Button("Play Again", action: reset)
        } message: {
            Text("Your score was \(score).")
        }
        
    }
    
    
    func play(choice: Int) {
        let winningMoves = [1, 2, 0]
        let didWin: Bool
        
        if playerShouldWin {
            didWin = choice == winningMoves[computerChoice]
        } else {
            didWin = winningMoves[choice] == computerChoice
        }

        if didWin {
            score += 1
        } else {
            score -= 1
        }
        
        if questionCount == 10 {
            showingResults = true
            
        } else {
            computerChoice = Int.random(in: 0..<3)
            playerShouldWin.toggle()
            questionCount += 1
        }

    }
    
    func reset() {
        computerChoice = Int.random(in: 0..<3)
        playerShouldWin = Bool.random()
        questionCount = 0
        score = 0
        
        

    }
    

}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
