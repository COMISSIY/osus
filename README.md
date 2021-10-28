# osus

import pygame, random, math
pygame.init()

w, h = 800, 600
sc = pygame.display.set_mode((w, h))
pygame.display.set_caption("OSUS")
clock = pygame.time.Clock()
f1 = pygame.font.Font(None, 40)
whole_count = 1
catched = 0
pygame.mouse.set_visible(False)
rad = 15
class Circle:
	def __init__(self, pos=[w//2, h//2], speed=2, n=0):
		global whole_count
		whole_count += 1
		self.anim = 100
		self.rad = 25
		self.pos = pos
		self.n = n
		self.speed = random.randint(10, 30)/10
	def draw(self):
		text = f1.render(str(self.n), 1, (255, 255, 255))
		text_collide = text.get_rect(center=self.pos)
		pygame.draw.circle(sc, (100, 100, 100), self.pos, self.rad, 1)
		pygame.draw.circle(sc, (255-self.speed, 100, 100), self.pos, self.anim, 1)
		sc.blit(text, text_collide)
	def update(self):
		self.anim -= self.speed
		if self.anim < self.rad and self not in del_list:
			del_list.append(self)

	def cheak_collide(self):
		global catched
		m_p = pygame.mouse.get_pos()
		lenght = math.sqrt(((self.pos[0]-m_p[0])**2+(self.pos[1]-m_p[1])**2))
		if lenght < self.rad and self not in del_list:
			del_list.append(self)
			print("Yep")
			catched += 1

circles = []
m_poses = []
del_list = []
circles.append(Circle())
while True:
	sc.fill((0, 0, 0))
	clock.tick(60)
	m_poses.append(pygame.mouse.get_pos())
	for i in pygame.event.get():
		if i.type == pygame.QUIT:
			print(catched)
			exit()
		if i.type == pygame.MOUSEBUTTONDOWN or i.type == pygame.KEYUP:
			for i in circles:
				i.cheak_collide()
			rad = 20
		else:
			if rad > 15:
				rad -= 2
	if len(m_poses) > 20:
		del m_poses[0]
	for n, i in enumerate(m_poses):
		pygame.draw.line(sc, (155+n, 155+n, 155+n), m_poses[n-1], i, n)
	for i in circles:
		i.draw()
		i.update()
	pygame.draw.circle(sc, (255, 250, 250), pygame.mouse.get_pos(), rad)
	for i in del_list:
		circles.remove(i)
	del_list = []
	if len(circles) < 1:
		circles.append(Circle(pos=[random.randint(25, w-25), random.randint(25, h-25)], n=len(circles)+1))
	score = f1.render(str(catched), 1, (255, 255, 255))
	accurate = f1.render(str((catched/whole_count)*10)[0:4], 1, (255, 255, 255))
	sc.blit(score, (0, 0))
	sc.blit(accurate, (0, 30))
	pygame.display.update()
