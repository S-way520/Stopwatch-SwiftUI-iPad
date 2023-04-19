# Stopwatch-SwiftUI-iPad
Stopwatch, SwiftUI + Only iPad + swift playgrounds 
![IMG_0843](https://user-images.githubusercontent.com/95877651/232965559-e8b4f0f9-dd60-42e7-b532-3786a23694eb.jpeg)

[Document 1]:

import SwiftUI

@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

[Document 2]:

import SwiftUI

struct ContentView: View {
    var body: some View {
        ClockView()
    }
}

struct ClockView: View {
    @State var isRunning = false
    @State var seconds = 0.0
    
    private let timer = Timer.publish(every: 1, on: .main, in: .default).autoconnect()
    
    var body: some View {
        VStack {
            ZStack {
                Circle()
                    .fill(Color.white)
                    .frame(width: 350, height: 350)
                //2 to 59, red and black
                ZStack {
                    ForEach(1..<31, id: \.self) { hour in
                        if hour % 2 == 0 {
                            Text("\(hour)")
                                .font(.system(size: 20))
                                .foregroundColor(.black)
                                .position(CGPoint(
                                    x: 150 * cos(CGFloat(hour) / 30 * CGFloat.pi * 2 - CGFloat.pi / 2), 
                                    y: 150 * sin(CGFloat(hour) / 30 * CGFloat.pi * 2 - CGFloat.pi / 2)
                                ))
                        }
                        else {
                            Text("\(hour+30)")
                                .font(.system(size: 20))
                                .foregroundColor(.red)
                                .position(CGPoint(
                                    x: 150 * cos(CGFloat(hour) / 30 * CGFloat.pi * 2 - CGFloat.pi / 2), 
                                    y: 150 * sin(CGFloat(hour) / 30 * CGFloat.pi * 2 - CGFloat.pi / 2)
                                ))
                        }
                    }
                }.frame(width: 30, height: 1).offset(x: 15)
                //1 to 15, red
                ZStack {
                    ForEach(1..<16, id: \.self) { hour in
                        Text("\(hour)")
                            .font(.system(size: 10))
                            .foregroundColor(.red)
                            .position(CGPoint(
                                x: 50 * cos(CGFloat(hour) / 15 * CGFloat.pi * 2 - CGFloat.pi / 2), 
                                y: 50 * sin(CGFloat(hour) / 15 * CGFloat.pi * 2 - CGFloat.pi / 2)
                            ))
                    }
                }.frame(width: 30, height: 1).offset(x: 15, y: -70)
                ForEach(0..<30, id: \.self) { index in
                    Rectangle()
                        .fill(Color.black)
                        .frame(width: 2, height: 10)
                        .offset(y: -175)
                        .rotationEffect(Angle(degrees: 6))
                        .rotationEffect(Angle(degrees: Double(index) * 12))
                }
                ForEach(0..<30, id: \.self) { index in
                    Rectangle()
                        .fill(Color.black)
                        .frame(width: 3, height: 15)
                        .offset(y: -170)
                        .rotationEffect(Angle(degrees: Double(index) * 12))
                }
                ForEach(0..<16, id: \.self) { index in
                    Rectangle()
                        .fill(Color.black)
                        .frame(width: 2, height: 5)
                        .offset(y: -60)
                        .rotationEffect(Angle(degrees: Double(index) * 24))
                }.offset(y: -70)
                ForEach(0..<16, id: \.self) { index in
                    Rectangle()
                        .fill(Color.black)
                        .frame(width: 1, height: 3)
                        .offset(y: -61)
                        .rotationEffect(Angle(degrees: 12))
                        .rotationEffect(Angle(degrees: Double(index) * 24))
                }.offset(y: -70)
                //
                Rectangle()
                    .fill(Color.black)
                    .frame(width: 4, height: 60)
                    .offset(y: -30)
                    .rotationEffect(Angle(degrees: seconds * 12 / 30))
                    .offset(y: -70)
                //
                Rectangle()
                    .fill(Color.red)
                    .frame(width: 2, height: 140)
                    .offset(y: -70)
                    .rotationEffect(Angle(degrees: seconds * 12))
                Button(action: {
                    self.isRunning.toggle()
                }) {
                    if isRunning {
                        Text("Stop")
                            .padding()
                            .background(Color.red)
                            .foregroundColor(.white)
                            .cornerRadius(10)
                    } else {
                        Text("Start")
                            .padding()
                            .background(Color.green)
                            .foregroundColor(.white)
                            .cornerRadius(10)
                    }
                }
                .offset(y: 230)
                
            }
        }
        .onReceive(timer) { _ in
            if isRunning {
                self.seconds += 1
            } else {
                self.seconds = 0.0
            }
        }
    }
}
