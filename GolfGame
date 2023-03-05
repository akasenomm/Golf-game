import pygame
from math import *
from sys import exit
import time
from random import *

#mängu eesmärk lüüa pall lipuga tähistatud auku.
#rajad peale esimest on juhuslikult kas pikad või lühikesed.
#pikkadel radadel võib lipp olla künka otsas.
#lipu kaugus stardipositsioonist on juhuslik.

#setup
pygame.init()
pygame.display.set_caption("Golfi mäng")
clock = pygame.time.Clock()
font = pygame.font.Font("Grand9K Pixel.ttf", 60)
font2 = pygame.font.Font("Grand9K Pixel.ttf", 20)
tick_counter = 180
score = 0
lööke = -1
#

#screenid, pildid jne
screen = pygame.display.set_mode((800, 400))
title = font.render("Golfi mäng", False, "Dark Green")
subtitle = font2.render("klikka, et mängida!", False, "Dark Green")
scoretext = font2.render("Skoor = "+str(score), False , "White")
lööketext = font2.render("Lööke = "+str(lööke), False, "White")

taevas_2 = pygame.image.load("taevas1.jpg")
taevas_3 = pygame.image.load("taevas2.jpg")
kyngas = pygame.image.load("kyngas.png")
mehike = pygame.image.load("mehike1.png")

lipp = pygame.image.load("lipp.png")
pilv = pygame.image.load("pilv.png")
starting = pygame.image.load( "starting.jpg")

mehike_copy = mehike.copy()
mehike_flipitud = pygame.transform.flip(mehike_copy, True, False)
mehike_lööb = [mehike]
koik_taevad = [taevas_2, taevas_3]

#
class ball(object): #viide: https://github.com/techwithtim/Golf-Game/blob/master/physics.py
    def __init__(self,x,y,radius,color):
        self.x = x
        self.y = y
        self.radius = radius
        self.color = color

    def draw(self, win): #palli joonistamise funktsioon (erinevalt nt lipust jm elementidest pole pall mitte pilt, vaid joonistatud pygamega)
        pygame.draw.circle(screen, (0,0,0), (self.x,self.y), self.radius)
        pygame.draw.circle(screen, self.color, (self.x,self.y), self.radius-1)
        
            

#
def ballPath(startx, starty, power, ang, time): #palli asukoht igas kaadris
    angle = ang
    velx = cos(angle) * power
    vely = sin(angle) * power

    distX = velx * time
    distY = (vely * time) + ((-4.9 * (time ** 2)) / 2)

    newx = round(distX + startx)
    newy = round(starty - distY)

    return (newx, newy)

#
def redrawWindow(pilt):
    screen.blit(pilt, (0,0)) #joonistab tausta
    golfBall.draw(screen) #joonistab palli
    
#
def findAngle(pos): #nurk kliki ja palli positsiooni vahel
    sX = golfBall.x
    sY = golfBall.y
    try:
        angle = atan((sY - pos[1]) / (sX - pos[0])) #pos[0] on kliki x ja pos[1] kliki y koordinaat
    except:
        angle = pi / 2

    if pos[1] < sY and pos[0] > sX:
        angle = abs(angle)
    elif pos[1] < sY and pos[0] < sX:
        angle = pi - angle
    elif pos[1] > sY and pos[0] < sX:
        angle = pi + abs(angle)
    elif pos[1] > sY and pos[0] > sX:
        angle = (pi * 2) - angle

    return angle

#
def standard(x, y, a, b): #joonistab mehikese, lipu, loendurid
    mehike_lööb = mehike
    screen.blit(lipp, (a, b))
    screen.blit(scoretext, (10, 1))
    screen.blit(lööketext, (10, 25))

#
def pall_start(x, y): #palli asukohta peale löögi lõppu (kui pall "maandunud")
    golfBall.x = x
    golfBall.y = y

#
def level(skoor, luger): #leveli setup
    global count5
    global tick_counter
    global mehike
    global mehike_flipitud
    global lipp
    global kyngas
    global koik_taevad_choice
    global lipp_random
    global score
    global count5
    global kyngas_random
    global koik_kynkad_choice
    global lööke
    
    
    if score == skoor:
        if count5 == luger:
            lipp_random = randint(400, 700)
            kyngas_random = randint(300, 600)
            count5 += 1
            luger += 1
            skoor += 1
            koik_taevad_choice = choice(koik_taevad)
            koik_kynkad_choice = randint(1, 2)
                
        redrawWindow(koik_taevad_choice)
        if koik_taevad_choice == taevas_3: #kui pikem level
            mehike = pygame.transform.scale(mehike, (50, 50))
            mehike_flipitud = pygame.transform.scale(mehike_flipitud, (50, 50))
            lipp = pygame.transform.scale(lipp, (50, 50))
            tick_counter = 165 #90
            if koik_kynkad_choice == 1: #kui pikemas levelis ka küngas
                screen.blit(kyngas, (kyngas_random-150, 210))
                kyngas_olemas = True
                if kyngas_olemas == True:
                    lipp_random = kyngas_random
                    kyngas = pygame.transform.scale(kyngas, (300, 150))
                    standard(10, 255, lipp_random-25, 175)
                    if not shoot:
                        if rect_lipp.x < golfBall.x: #kui pall on lipust paremal
                            if rect_lipp.x-golfBall.x > -120:
                                screen.blit(mehike_flipitud, (golfBall.x-30, golfBall.y-48))
                            else:
                                screen.blit(mehike_flipitud, (golfBall.x-10, golfBall.y-48))
                        else:
                            if rect_lipp.x-golfBall.x < 100: #kui pall on lipust vasakul
                                screen.blit(mehike, (golfBall.x-27, golfBall.y-48)) 
                            else:
                                screen.blit(mehike, (golfBall.x-10, golfBall.y-48))
                            
            else:
                standard(10, 255, lipp_random-40, 255)
                if not shoot:
                    if rect_lipp.x < golfBall.x :
                        screen.blit(mehike_flipitud, (golfBall.x-35, golfBall.y-48))
                    else:
                        screen.blit(mehike, (golfBall.x-14, golfBall.y-48))
        else: #lühike level
            mehike = pygame.transform.scale(mehike, (100, 100))
            mehike_flipitud = pygame.transform.scale(mehike_flipitud, (100, 100))
            lipp = pygame.transform.scale(lipp, (100, 100))
            standard(10, 210, lipp_random-40, 210)
            tick_counter = 165
            if not shoot:
                if rect_lipp.x < golfBall.x :
                    screen.blit(mehike_flipitud, (golfBall.x-75, golfBall.y-95))
                else:
                    screen.blit(mehike, (golfBall.x-28, golfBall.y-95))

            

#booleanid ja muu
golfBall = ball(25,310,5,(255,255,255))
koik_taevad_choice = taevas_2
koik_kynkad_choice = False
start_screen = False
shoot = False
power = False
angle = False
count5 = False
run = True
kyngas_random = False
lipp_random = False
rect_lipp = pygame.Rect(lipp_random+15, 290, 40, 30)

while run:
    rect_pall = pygame.Rect(golfBall.x-5, golfBall.y-5, 10, 10) #palli hitbox
    if koik_taevad_choice == taevas_2: #kui lühike level
        rect_lipp = pygame.Rect(lipp_random-5, 290, 10, 5) #lipu hitbox
    else:
        rect_lipp = pygame.Rect(lipp_random-10, 290, 10, 5)
    
    if koik_taevad_choice == taevas_3 and koik_kynkad_choice == 1: #kui pikem level ja küngas
        rect_kyngas = pygame.Rect(kyngas_random-50, 225, 100, 80)
        rect_lipp = pygame.Rect(lipp_random-10, 225, 20, 10)
        
        if rect_pall.colliderect(rect_kyngas): #pall põrkab künka vastu
            if golfBall.y <= rect_kyngas.y:
                pall_start(golfBall.x, rect_kyngas.y-5)
            
            else:
                if golfBall.x < 1.05*rect_kyngas.x: #et pall ei jääks künka sisse, vaid oleks nö täpselt künka peal (kui pall on lipust vasakul)
                    if golfBall.y > 280:
                        pall_start(golfBall.x-30, golfBall.y)
                    elif golfBall.y > 260:
                        pall_start(golfBall.x-20, golfBall.y)
                    elif golfBall.y > 240:
                        pall_start(golfBall.x-10, golfBall.y)
                    else:
                        pall_start(golfBall.x-5, golfBall.y)
                else: #et pall ei jääks künka sisse, kui pall on lipust paremal
                    if golfBall.y > 280:
                        pall_start(golfBall.x+30, golfBall.y)
                    elif golfBall.y > 260:
                        pall_start(golfBall.x+20, golfBall.y)
                    elif golfBall.y > 240:
                        pall_start(golfBall.x+10, golfBall.y)
                    else:
                        pall_start(golfBall.x+5, golfBall.y)
    
            shoot = False
            t = 0
            if rect_pall.colliderect(rect_lipp): #pall sees 
                score += 1
                scoretext = font2.render("Skoor = "+str(score), False , "White")
                pall_start(15, 300)
                time.sleep(0.05)
                shoot = False
        
        
    pygame.display.flip()

    clock.tick(tick_counter)
    if start_screen != True: #starting screen
        screen.blit(starting, (0,0))
        screen.blit(title, (100, 50))
        screen.blit(subtitle, (150, 150))
        lipp_random = 700
        kyngas_random = 500
    else:
        redrawWindow(taevas_2)
        standard(10, 210, 650, 210)
        if not shoot:
            screen.blit(mehike_lööb[0], (golfBall.x, golfBall.y-90))
                
    #kui pall õhus    
    if shoot:
        if golfBall.y < 310 - golfBall.radius:
            t += 0.1
            po = ballPath(x, y, power, angle, t)
            golfBall.x = po[0]
            golfBall.y = po[1]
        else:
            time.sleep(0.05)
            shoot = False
            t = 0
            golfBall.y = 300

    #pall sees - lühike lvl        
    if golfBall.x < (lipp_random + 15) and golfBall.x > (lipp_random - 15): 
        if golfBall.y == 300:
            print("Sees", golfBall.x, golfBall.y)
            score += 1
            print (score)
            scoretext = font2.render("Skoor = "+str(score), False , "White")
            shoot = False
            pall_start(15, 300)
            
    line = [(golfBall.x, golfBall.y), pygame.mouse.get_pos()]  #vektor palli ja hiire koordinaatide vahel
    for event in pygame.event.get():
        if event.type == pygame.MOUSEBUTTONDOWN: #klikk
            if event.button == 1:
                lööke += 1
                lööketext = font2.render("Lööke = "+str(lööke), False, "White")
                if not shoot:
                    start_screen = True
                    shoot = True
                    x = golfBall.x
                    y = golfBall.y
                    pos =pygame.mouse.get_pos()
                    angle = findAngle(pos)
                    power = min((sqrt((line[1][1]-line[0][1])**2 +(line[1][0]-line[0][0])**2)/3), 60)
                    if koik_taevad_choice == taevas_3: #et töötaks pikemal levelil
                        power = power/1.3
                
        if event.type == pygame.QUIT: #mängu sulgemine
            pygame.quit()
            exit()
            
    #levelid
    level(1, 0)
    for i in range(100): #argument levelite arv
        level(i+1, i)


    ###
    pygame.display.update()

