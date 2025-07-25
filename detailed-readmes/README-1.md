# Project-KNN (1o Παραδοτέο)

[Θεοδοσία Παπαδήμα](https://github.com/sulpap)

[Γιώργος Κορύλλος](https://github.com/GeorgeKorillos)

[Βασιλική Τσαντήλα](https://github.com/VassTs)

### Οδηγίες εκτέλεσης
Για να τρέξετε την main:
* Μεταφέρεστε στο directory που βρίσκεται το Makefile.
* Εκτελείτε την εντολή: make
* Εκτελείτε την εντολή: chmod +x scripts/run_first_main.sh
* Εκτελείτε την εντολή: ./scripts/run_first_main.sh

Για να τρέξετε τα test:
* Μεταφέρεστε στο directory που βρίσκεται το Makefile.
* Εκτελείτε την εντολή: make test
* Εκτελείτε την εντολή: ./bin/test

Για να καθαρίσετε τα αντικείμενα και εκτελέσιμα αρχεία:
* Στο directory που βρίσκεται το Makefile, εκτελείτε την εντολή: make clean

### Διαχωρισμός εργασιών

Το κάθε μέλος ανέλαβε από έναν βασικό αλγόριθμο, καθώς και από έναν αριθμό επιμέρους βοηθητικών συναρτήσεων. Ο καθένας υλοποίησε τα unit tests που αντιστοιχούν στις υλοποιήσεις του.

### Σχεδιαστικές επιλογές

- Υλοποιήθηκε μια πλήρης δομή γράφου (με χρήση adjacency list) με όλες τις λειτουργικότητες που χρειάζονται. Ο γράφος αναπαρίσταται με ένα map, τα keys του οποίου είναι (int) nodes-ids, και τα values του οποίου είναι list<Node*\>. Προκειμένου η δομή του γράφου να είναι γενικής χρήσης, κάθε instance ενός γράφου χαρακτηρίζεται και από ένα id. Σημειώνουμε ότι τα ids των γράφων ξεκινούν από 1.     
Ο κόμβος (class Node) αποτελείται από: ένα id, vector<double> coordinates, και list<Node*> edges. Τα ids των κόμβων ξεκινούν από 0. Σημειώνουμε ότι κόμβοι με id γράφου 0 δεν ανήκουν σε γράφο.

- Στο αρχείο utility βρίσκονται οι συναρτήσεις για τον υπολογισμό των αποστάσεων, του medoid και μετατροπής ενός vector of vectors που περιέχει floats σε ένα vector of vectors που περιέχει double.

- Η συνάρτηση εύρεσης του medoid (findMedoid), υλοποιήθηκε με τον ζητούμενο "παραδοσιακό" τρόπο, με μια παραλλαγή ότι υπολογίζει σε ζεύγη τις αποστάσεις, χωρίς να κάνει διπλούς υπολογισμούς, κερδίζοντας, έτσι, πολύ χρόνο (η κλασσική μέθοδος είναι υλοποιημένη σε σχόλια).

- Στους αλγορίθμους GreedySearch, RobustPrune και Vamana ακολουθήθηκε ο ψευδοκώδικας της εκφώνησης.

- Ο αλγόριθμος Vamana χρησιμοποιεί μια συνάρτηση γεννήτρια γράφου, η οποία προσθέτει R τυχαίες ακμές σε κάθε κόμβο. Επίσης, επιστρέφει την τιμή του medoid, η οποία θα χρειαστεί ώστε να κληθεί σωστά ο GreedySearch στην main.

- O Vamana δίνει στην main έναν γράφο του οποίου οι κόμβοι είναι συνδεδεμένοι με τους κοντινότερους γείτονές τους (το πολύ R) και στη συνέχεια η main καλεί τον GreedySearch για κάθε δοσμένο query με start node medoid, ώστε να βρει τους k κοντινότερους γείτονές του στον γράφο αυτό.
  
- Σχεδιαστικές επιλογές greedy algorithm:  
Μελετώντας τον ψευδοκώδικα του greedysearch, παρατηρούμε ότι είναι αναγκαίες οι πράξεις μεταξύ συνολών. 
Παρατηρούμε ότι τα σύνολα πάνω στα οποία θα εφαρμοστούν αυτές οι πράξεις περιέχουν σημεία, τα οποία στην υλοποίηση μας αναπαριστούμε ως κόμβους (Node*). Προσδιορίζουμε τη μοναδικότητα ενός node σύμφωνα με τη διεύθυνση του, γι' αυτό και αποφασίζουμε σε κάθε set να αποθηκεύουμε δείκτες σε node.    
Χρησιμοποιούμε το container set της c++, και αντίστοιχα τη συνάρτηση set_difference της βιβλιοθήκης algrorithm για τη διαφορά ανάμεσα σε δύο σύνολα.    
Παρατηρούμε ότι η ένωση δύο sets, μπορεί να πραγματοποιηθεί και με την set.insert η οποία και μάς αρκεί.     
Δεδομένου ότι τα περιεχόμενα ενός set στην περίπτωση μας είναι sorted σε αύξουσα σειρά με βάση Node*, αν θέλουμε να αλλάξουμε το μέγεθος του set ώστε να περιέχει εκείνα τα nodes που είναι πιο κοντά στο x_q: αντιγράφουμε τα περιεχόμενα του set σε ένα προσωρινό vector. Κάνουμε sort το vector σύμφωνα με την απόσταση του κάθε node από το x_q, και τέλος ενημερώνουμε το αρχικό set με τα κατάλληλα nodes.   

### Αποτελέσματα & Χρόνοι (Παραδείγματα)

Δίνοντας την siftsmall βάση και τις παρακάτω τιμές σε δύο διαφορετικούς υπολογιστές και στα linux της σχολής έχουμε τους εξής χρόνους:

* PC 1 (Native Linux. CPU: 11th Gen Intel® Core™ i7-1165G7 @ 2.80GHz × 8):
  * k = 20, L = 40, R = 80, a = 2.0: 964.066 seconds or 16.0678 minutes με accuracy 99.6%
  * k = 100, L = 200, R = 60, a = 2.0: 640.234 seconds or 10.6706 minutes με accuracy 97.89%

* PC 2 (Linux με Virtual Machine. CPU: Intel(R) Core(TM) i5-6500 CPU @ 3.20GHz):
  * k = 20, L = 40, R = 80, a = 2.0: 1428.65 seconds or 23.8109 minutes με accuracy 99.9%
  * k = 100, L = 200, R = 60, a = 2.0: 1085.12 seconds or 18.0853 minutes με accuracy 98.84%
  * k = 100, L = 100, R = 40, a = 1.2:  212.743 seconds or 3.54571 minutes με accuracy 96.56%
  * k = 100, L = 200, R = 60, a = 1.2: 649.905 seconds or 10.8318 minutes με accuracy 99.05%
  * k = 20, L = 40, R = 80, a = 1.2: 73.2621 seconds or 1.22104 minutes με accuracy 80.15%

* PC 3 (Native Linux. CPU: 13th Gen Intel Core i5-13500 @ 20x 4,8GHz):
  * k = 20, L = 40, R = 80, a = 2.0: 345.027 seconds or 5.75044 minutes με accuracy 99.9%
  * k = 100, L = 200, R = 60, a = 2.0: 265.741 seconds or 4.42901 minutes με accuracy 98.86%
  * k = 100, L = 100, R = 40, a = 1.2: 56.4917 seconds or 0.941529 minutes με accuracy 96.48%
  * k = 100, L: 200, R = 60, a = 1.2: 181.499 seconds or 3.02498 minutes με auracy 99.32%
  * k = 20, L = 40, R = 80, a = 1.2: 16.0143 seconds or 0.266904 minutes με accuracy 80.25%
 
* Linux Σχολής (linux15.di.uoa.gr):
  * k = 20, L = 40, R = 80, a = 2.0: 871.887 seconds or 14.5314 minutes με accuracy 99.9%
  * k = 100, L = 200, R = 60, a = 2.0: 646.799 seconds or 10.78 minutes με accuracy 98.88%
  * k = 100, L = 100, R = 40, a = 1.2: 139.628 seconds or 2.32714 minutes με accuracy 96.96%
  * k = 100, L: 200, R = 60, a = 1.2: 447.556 seconds or 7.45926 minutes με accuracy 99.27%
  * k = 20, L = 40, R = 80, a = 1.2: 41.6668 seconds or 0.694446 minutes με accuracy 81.3%

Δεν υπάρχουν διαρροές μνήμης.