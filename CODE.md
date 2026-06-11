Voici le lien Github de mon code de filtrage numérique de la tension induite de la machine à courant continu









import numpy as np
import matplotlib.pyplot as plt


Te=0.1 
K=1 
w0=5 


def lecture_fichier(mesuretension):
    fichier=open(mesuretension,'r')
    temps,tension=[],[]
    for line in fichier:
        instant,t=line.split(';')
        temps.append(float(instant.replace(',','.')))
        tension.append(float(t.replace(',','.')))
    fichier.close()
    return temps,tension

temps,tension=lecture_fichier('mesuretension.csv')

def filt_num_pb1(entree,Te,w0,K):
 
    nb_elt=len(entree)
    sortie=np.zeros(nb_elt)
    sortie[0]=K*entree[0]
    a0= (K*Te*w0)/(1+Te*w0)
    a1= 1/(1+Te*w0)
    for n in range(1,nb_elt):
        sortie[n]= a0 * entree[n] + a1 * sortie[n-1]
    return sortie


sortiefiltpb1= filt_num_pb1(tension, Te, w0, K)


plt.figure()
plt.plot(temps,tension,label='Données brutes')
plt.plot(temps, sortiefiltpb1)
plt.xlabel('Temps [s]')
plt.ylabel('tension [V]')
plt.title('Tracés de la tension et filtrée par filtre passe-bas d\'ordre 1')
plt.grid()
plt.legend()
plt.axis([0,20,0,250])

