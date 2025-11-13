# oving

def hovedmeny():
    emner, studieplaner = les_data()

    while True:
        print("""
============================
MENY
1. Lag nytt emne
2. Legg til emne i studieplan
3. Fjern emne fra studieplan
4. Skriv ut alle emner
5. Lag ny studieplan
6. Skriv ut studieplan
7. Sjekk om studieplan er gyldig
8. Finn hvilke studieplaner som bruker et emne
9. Lagre data til fil
10. Les inn data fra fil
11. Avslutt
============================
""")
        valg = input("Velg et menyvalg: ")

        if valg == "1":
            kode = input("Emnekode: ").upper()
            navn = input("Emnenavn: ")
            semester = input("Semester (H/V): ").upper()
            sp = int(input("Studiepoeng: "))
            emner.append(Emne(kode, navn, semester, sp))
            print("✅ Nytt emne lagt til.")

        elif valg == "2":
            if not studieplaner:
                print("Ingen studieplaner opprettet enda.")
                continue
            print("Tilgjengelige planer:")
            for sp in studieplaner:
                print(f"{sp.plan_id}: {sp.tittel}")
            pid = input("Velg plan-id: ")
            plan = next((s for s in studieplaner if s.plan_id == pid), None)
            if not plan:
                print("Fant ikke studieplanen.")
                continue
            print("Tilgjengelige emner:")
            for e in emner:
                print(f"- {e}")
            kode = input("Hvilket emne skal legges til? ").upper()
            emne = next((e for e in emner if e.kode == kode), None)
            if not emne:
                print("Fant ikke emne.")
                continue
            sem = int(input("Semester (1–6): "))
            plan.legg_til_emne(emne, sem)

        elif valg == "3":
            pid = input("Plan-id: ")
            plan = next((s for s in studieplaner if s.plan_id == pid), None)
            if not plan:
                print("Fant ikke studieplan.")
                continue
            kode = input("Emnekode å fjerne: ").upper()
            plan.fjern_emne(kode)

        elif valg == "4":
            for e in emner:
                print(e)

        elif valg == "5":
            pid = input("Ny plan-id: ")
            tittel = input("Tittel: ")
            studieplaner.append(Studieplan(pid, tittel))
            print("✅ Ny studieplan opprettet.")

        elif valg == "6":
            pid = input("Plan-id: ")
            plan = next((s for s in studieplaner if s.plan_id == pid), None)
            if plan:
                plan.skriv_ut()
            else:
                print("Fant ikke studieplan.")

        elif valg == "7":
            pid = input("Plan-id: ")
            plan = next((s for s in studieplaner if s.plan_id == pid), None)
            if plan:
                plan.er_gyldig()
            else:
                print("Fant ikke studieplan.")

   
        elif valg == "8":
            kode = input("Emnekode: ").upper()
            brukte = []
            for sp in studieplaner:
                # Sjekk om emnet brukes i noen semester i studieplanen
                if any(e.kode == kode for emner in sp.semestre.values() for e in emner):
                    brukte.append(sp.tittel)

            if brukte:
                print("Emnet brukes i:", ", ".join(brukte))
            else:
                print("Emnet brukes ikke i noen studieplaner.")

        elif valg == "9":
            lagre_data(emner, studieplaner)

        elif valg == "10":
            emner, studieplaner = les_data()

        elif valg == "11":
            print("Avslutter...")
            break

        else:
            print("Ugyldig valg, prøv igjen.")



if __name__ == "__main__":
    hovedmeny()


