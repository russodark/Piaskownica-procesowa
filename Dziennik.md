Dziennik projektu — Temat 1: Piaskownica procesowa w Linuksie
(przestrzenie nazw, cgroups i seccomp)

Autor: Roman Akhunjanov
Numer indeksu: 71691

Przygotowanie maszyny wirtualnej 

Co zrobiłem:

Zainstalowałem darmowy program VirtualBox ze strony virtualbox.org.

Pobrałem obraz Ubuntu 24.04 (desktop) z ubuntu.com (plik .iso).

Utworzyłem maszynę wirtualną z parametrami: 2 procesory, 4 GB RAM, 20 GB dysku.

W ustawieniach maszyny podłączyłem obraz .iso do napędu i uruchomiłem instalację.

Postępowałem zgodnie z instalatorem. Zapisałem nazwę użytkownika i hasło.

Wynik: Maszyna wirtualna uruchomiona, system Ubuntu 24.04 zainstalowany.

Sprawdzenie łączności z internetem 

Co zrobiłem:

W maszynie wirtualnej otworzyłem terminal (Ctrl+Alt+T).

Wykonałem polecenie: sudo apt update

Wynik: Komenda pobrała listy pakietów z internetu bez błędów. Środowisko jest gotowe do dalszej pracy.

Pierwszy eksperyment izolacyjny 

Co zrobiłem:

W terminalu wykonałem polecenie: unshare --pid --fork --mount-proc bash

Następnie w nowej powłoce wykonałem: ps aux

Co zaobserwowałem:

Widoczne były tylko dwa procesy — bash i ps.

Było to tak, jakby na komputerze nie działo się nic innego.

To właśnie jest przestrzeń nazw (namespace) — proces widzi tylko własny, osobny świat.

Co mnie zaskoczyło:

Że izolacja PID działa tak prosto — jedno polecenie i proces nie widzi reszty systemu.

Że --mount-proc jest konieczne, aby ps w ogóle działał poprawnie w nowej przestrzeni.

Zakończenie:

Nacisnąłem exit, aby wrócić do normalnej powłoki.

Wynik: Eksperyment powtórzony samodzielnie, obserwacja zapisana.

Ograniczenie zasobów 

Co zrobiłem:

Wykonałem polecenie: systemd-run --user --scope -p MemoryMax=100M bash

W nowej powłoce uruchomiłem program, który próbuje zjeść całą pamięć:
yes | head -c 500M > /dev/null

Co zaobserwowałem:

System nie pozwolił przekroczyć limitu 100 MB pamięci.

Proces został zatrzymany przez mechanizm cgroups.

Otrzymałem komunikat o przekroczeniu limitu pamięci.

Zakończenie:

Nacisnąłem Ctrl+C, aby przerwać proces.

Następnie exit, aby wyjść z powłoki.

Wynik: Potwierdzone działanie mechanizmu cgroups — limit pamięci jest egzekwowany.

Podział ról i plan

Wszystkie zadania i role w projekcie wykonał samodzielnie:
Roman Akhunjanov (nr indeksu: 71691)

Plan na najbliższe dwa tygodnie:

Tydzień 1: Dokończenie eksperymentów z seccomp, analiza wyników, uzupełnienie dziennika.

Tydzień 2: Przygotowanie prezentacji, zebranie wniosków, oddanie projektu do repozytorium.

Wnioski

Przestrzenie nazw (namespaces) pozwalają na izolację procesów — proces widzi tylko swój własny świat.

cgroups umożliwiają ograniczanie zasobów (pamięci, CPU) dla grup procesów.

seccomp (do zbadania w kolejnych zajęciach) pozwala filtrować wywołania systemowe.

Wszystkie te mechanizmy razem tworzą podstawę konteneryzacji.



