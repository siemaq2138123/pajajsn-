package com.company;

import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.util.Timer;
import java.util.TimerTask;
import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JEditorPane;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextField;

public class Main
{

    public static final int wysokoscstolu = karta.wysokockarty * 4;
    public static final int szerokoscstolu = (karta.szerokosckarty * 7) + 100;
    public static final int koncowyzestaw = 4;
    public static final int liczbazestawow = 7;
    public static final Point polozeniezestawu = new Point(5, 5);
    public static final Point pokapolozenie = new Point(polozeniezestawu.x + karta.szerokosckarty + 5, polozeniezestawu.y);
    public static final Point ostatecznepolozenie = new Point(pokapolozenie.x + karta.szerokosckarty + 150, polozeniezestawu.y);
    public static final Point polozeniestart = new Point(polozeniezestawu.x, ostatecznepolozenie.y + karta.wysokockarty + 30);

    private static osattki[] koncowekarty;
    private static karciochy[] kartydogry;
    private static final karta polozenienowejkarty = new karta();
    private static karciochy zestaw;


    private static final JFrame ramka = new JFrame("Pasjans pająk wersja deluxe");
    protected static final JPanel stolik = new JPanel();

    private static JEditorPane tytulgry = new JEditorPane("text/html", "");
    private static JButton przyciskzasad = new JButton("poka zasady");
    private static JButton nowynawcisk = new JButton("Nowa gierka");
    private static JButton nawciskczasu = new JButton("zatrzymaj czas");
    private static JButton nawciskkonca = new JButton("zakończ gre");
    private static JTextField wynik1 = new JTextField();
    private static JTextField czas1 = new JTextField();
    private static JTextField statystyki = new JTextField();
    private static final karta nowynawciskkarty = new karta();


    private static Timer czasownik = new Timer();
    private static ScoreClock wynikzegara = new ScoreClock();


    private static boolean czas2 = false;
    private static int wyniczek = 0;
    private static int czas = 0;




    protected static karta ruchkarta(karta c, int x, int y)
    {
        c.setBounds(new Rectangle(new Point(x, y), new Dimension(karta.szerokosckarty + 10, karta.wysokockarty + 10)));
        c.setXY(new Point(x, y));
        return c;
    }

    protected static void setWyniczek(int wynikpocz)
    {
        Main.wyniczek += wynikpocz;
        String nowywynik = "WYNICZEK: " + Main.wyniczek;
        wynik1.setText(nowywynik);
        wynik1.repaint();
    }

    protected static void czaswonik()
    {
        Main.czas += 1;
        if (Main.czas % 10 == 0)
        {
            setWyniczek(-2);
        }
        String time = "czas: " + Main.czas;
        czas1.setText(time);
        czas1.repaint();
    }

    protected static void startTimer()
    {
        wynikzegara = new ScoreClock();
        czasownik.scheduleAtFixedRate(wynikzegara, 1000, 1000);
        czas2 = true;
    }

    protected static void toggleTimer()
    {
        if (czas2 && wynikzegara != null)
        {
            wynikzegara.cancel();
            czas2 = false;
        } else
        {
            startTimer();
        }
    }

    private static class ScoreClock extends TimerTask
    {
        @Override
        public void run()
        {
            czaswonik();
        }
    }

    private static class NewGameListener implements ActionListener
    {
        @Override
        public void actionPerformed(ActionEvent e)
        {
            nowagra();
        }

    }





    private static class ToggleTimerListener implements ActionListener
    {
        @Override
        public void actionPerformed(ActionEvent e)
        {
            toggleTimer();
            if (!czas2)
            {
                nawciskczasu.setText("CZAS START");
            } else
            {
                nawciskczasu.setText("ZATRZYMAJ CZAS");
            }
        }

    }
    private static class siema implements ActionListener{
        @Override
        public void actionPerformed(ActionEvent e){
            System.exit(0);
        }
    }

    private static class ShowRulesListener implements ActionListener
    {
        @Override
        public void actionPerformed(ActionEvent e)
        {
            JDialog ruleFrame = new JDialog(ramka, true);
            ruleFrame.setDefaultCloseOperation(JDialog.DISPOSE_ON_CLOSE);
            ruleFrame.setSize(wysokoscstolu, szerokoscstolu);
            JScrollPane scroll;
            JEditorPane rulesTextPane = new JEditorPane("TEKSCIK", "");
            rulesTextPane.setEditable(false);

            String rulesText = "ZASADY SĄ TAKIE JAK ZAWSZE I SIE NIE BOJ O TO ŻE PRZEGRASZ"
            + ("<br><br>nie boj sie mnie tak?");
            rulesTextPane.setText(rulesText);
            ruleFrame.add(scroll = new JScrollPane(rulesTextPane));

            ruleFrame.setVisible(true);
        }
    }

    private static class CardMovementManager extends MouseAdapter
    {
        private com.company.karta poprzedniakarta = null;
        private com.company.karta ruchkarty = null;
        private boolean ostetecznyzestaw = false;
        private boolean wymianazestawu = true;
        private boolean sprawdzeniewygranej = false;
        private boolean koniec = true;
        private Point start = null;
        private Point stop = null;
        private com.company.karta karta = null;

        private karciochy pochodzenie = null;
        private karciochy przyklad = null;

        private karciochy wymiana1 = new karciochy(false);

        private boolean wwa4(com.company.karta source, com.company.karta deska)
        {
            int s_val = source.getValue().ordinal();
            int d_val = deska.getValue().ordinal();
            com.company.karta.zestaww s_suit = source.getSuit();
            com.company.karta.zestaww d_suit = deska.getSuit();


            if ((s_val + 1) == d_val)
            {

                switch (s_suit)
                {
                    case pik:
                    case trefl:
                        if (d_suit != com.company.karta.zestaww.serce && d_suit != com.company.karta.zestaww.karo)
                            return false;
                        else
                            return true;
                    case serce:
                    case karo:
                        if (d_suit != com.company.karta.zestaww.pik && d_suit != com.company.karta.zestaww.trefl)
                            return false;
                        else
                            return true;
                }
            }
            return false;
        }

        private boolean ostetecznyruch(com.company.karta pochodnik, com.company.karta pochodnik1)
        {
            int s_val = pochodnik.getValue().ordinal();
            int d_val = pochodnik1.getValue().ordinal();
            com.company.karta.zestaww wwa = pochodnik.getSuit();
            com.company.karta.zestaww wwa1 = pochodnik1.getSuit();
            if (s_val == (d_val + 1))
            {
                if (wwa == wwa1)
                    return true;
                else
                    return false;
            } else
                return false;
        }

        @Override
        public void mousePressed(MouseEvent e)
        {
            start = e.getPoint();
            boolean koniecszukania = false;
            statystyki.setText("");
            wymiana1.makeEmpty();

            for (int x = 0; x < liczbazestawow; x++)
            {
                if (koniecszukania)
                    break;
                pochodzenie = kartydogry[x];
                for (Component ca : pochodzenie.getComponents())
                {
                    com.company.karta c = (com.company.karta) ca;
                    if (c.wwa3() && pochodzenie.contains(start))
                    {
                        wymiana1.pierwszy(c);
                    }
                    if (c.contains(start) && pochodzenie.contains(start) && c.wwa3())
                    {
                        karta = c;
                        koniecszukania = true;
                        System.out.println(": " + wymiana1.pokarozmiar());
                        break;
                    }
                }

            }
            if (nowynawciskkarty.contains(start) && zestaw.pokarozmiar() > 0)
            {
                if (wymianazestawu && poprzedniakarta != null)
                {
                    System.out.println("Powrót na stos pokazowy: ");
                    poprzedniakarta.getValue();
                    poprzedniakarta.getSuit();
                    zestaw.pierwszy(poprzedniakarta);
                }

                System.out.print("talia ");
                zestaw.pokarozmiar();
                if (poprzedniakarta != null)
                    stolik.remove(poprzedniakarta);
                com.company.karta c = zestaw.pop().pokazgora();
                stolik.add(Main.ruchkarta(c, pokapolozenie.x, pokapolozenie.y));
                c.repaint();
                stolik.repaint();
                poprzedniakarta = c;
            }

            if (polozenienowejkarty.contains(start) && poprzedniakarta != null)
            {
                ruchkarty = poprzedniakarta;
            }

            for (int x = 0; x < koncowyzestaw; x++)
            {

                if (koncowekarty[x].contains(start))
                {
                    pochodzenie = koncowekarty[x];
                    karta = pochodzenie.getLast();
                    wymiana1.pierwszy(karta);
                    ostetecznyzestaw = true;
                    break;
                }
            }
            wymianazestawu = true;

        }

        @Override
        public void mouseReleased(MouseEvent e)
        {
            stop = e.getPoint();

            boolean zlyruch = false;


            if (ruchkarty != null)
            {

                for (int x = 0; x < liczbazestawow; x++)
                {
                    przyklad = kartydogry[x];
                    if (przyklad.pusty() && ruchkarty != null && przyklad.contains(stop)
                            && ruchkarty.getValue() == com.company.karta.wartosc.krol)
                    {
                        System.out.print("Rusz nowa karte na puste miejsce ");
                        ruchkarty.setXY(przyklad.getXY());
                        stolik.remove(poprzedniakarta);
                        przyklad.pierwszy(ruchkarty);
                        stolik.repaint();
                        ruchkarty = null;
                        wymianazestawu = false;
                        setWyniczek(5);
                        zlyruch = true;
                        break;
                    }
                    if (ruchkarty != null && przyklad.contains(stop) && !przyklad.pusty() && przyklad.wwa5().wwa3()
                            && wwa4(ruchkarty, przyklad.wwa5()))
                    {
                        System.out.print("Ruszaj nową kartę ");
                        ruchkarty.setXY(przyklad.wwa5().getXY());
                        stolik.remove(poprzedniakarta);
                        przyklad.pierwszy(ruchkarty);
                        stolik.repaint();
                        ruchkarty = null;
                        wymianazestawu = false;
                        setWyniczek(5);
                        zlyruch = true;
                        break;
                    }
                }

                for (int x = 0; x < koncowyzestaw; x++)
                {
                    przyklad = koncowekarty[x];
                    if (przyklad.pusty() && przyklad.contains(stop))
                    {
                        if (ruchkarty.getValue() == com.company.karta.wartosc.as)
                        {
                            przyklad.pchanie(ruchkarty);
                            stolik.remove(poprzedniakarta);
                            przyklad.repaint();
                            stolik.repaint();
                            ruchkarty = null;
                            wymianazestawu = false;
                            setWyniczek(10);
                            zlyruch = true;
                            break;
                        }
                    }
                    if (!przyklad.pusty() && przyklad.contains(stop) && ostetecznyruch(ruchkarty, przyklad.getLast()))
                    {
                        System.out.println("Cel" + przyklad.pokarozmiar());
                        przyklad.pchanie(ruchkarty);
                        stolik.remove(poprzedniakarta);
                        przyklad.repaint();
                        stolik.repaint();
                        ruchkarty = null;
                        wymianazestawu = false;
                        sprawdzeniewygranej = true;
                        setWyniczek(10);
                        zlyruch = true;
                        break;
                    }
                }
            }


            if (karta != null && pochodzenie != null)
            {
                for (int x = 0; x < liczbazestawow; x++)
                {
                    przyklad = kartydogry[x];
                    if (karta.wwa3() == true && przyklad.contains(stop) && pochodzenie != przyklad && !przyklad.pusty()
                            && wwa4(karta, przyklad.wwa5()) && wymiana1.pokarozmiar() == 1)
                    {
                        com.company.karta c = null;
                        if (ostetecznyzestaw)
                            c = pochodzenie.pop();
                        else
                            c = pochodzenie.popFirst();

                        c.repaint();
                        if (pochodzenie.wwa5() != null)
                        {
                            com.company.karta temp = pochodzenie.wwa5().pokazgora();
                            temp.repaint();
                            pochodzenie.repaint();
                        }

                        przyklad.setXY(przyklad.getXY().x, przyklad.getXY().y);
                        przyklad.pierwszy(c);

                        przyklad.repaint();

                        stolik.repaint();

                        System.out.print("Miejsce docelowe ");
                        przyklad.pokarozmiar();
                        if (ostetecznyzestaw)
                            setWyniczek(15);
                        else
                            setWyniczek(10);
                        zlyruch = true;
                        break;
                    } else if (przyklad.pusty() && karta.getValue() == com.company.karta.wartosc.krol && wymiana1.pokarozmiar() == 1)
                    {
                        com.company.karta c = null;
                        if (ostetecznyzestaw)
                            c = pochodzenie.pop();
                        else
                            c = pochodzenie.popFirst();

                        c.repaint();
                        if (pochodzenie.wwa5() != null)
                        {
                            com.company.karta temp = pochodzenie.wwa5().pokazgora();
                            temp.repaint();
                            pochodzenie.repaint();
                        }

                        przyklad.setXY(przyklad.getXY().x, przyklad.getXY().y);
                        przyklad.pierwszy(c);

                        przyklad.repaint();

                        stolik.repaint();

                        System.out.print("Miejsce docelowe ");
                        przyklad.pokarozmiar();
                        setWyniczek(5);
                        zlyruch = true;
                        break;
                    }
                    if (przyklad.pusty() && przyklad.contains(stop) && !wymiana1.pusty()
                            && wymiana1.wwa5().getValue() == com.company.karta.wartosc.krol)
                    {
                        System.out.println("KRÓL JEST DO SKIPA");
                        while (!wymiana1.pusty())
                        {
                            System.out.println("POKAZ KARTY: " + wymiana1.wwa5().getValue());
                            przyklad.pierwszy(wymiana1.popFirst());
                            pochodzenie.popFirst();
                        }
                        if (pochodzenie.wwa5() != null)
                        {
                            com.company.karta temp = pochodzenie.wwa5().pokazgora();
                            temp.repaint();
                            pochodzenie.repaint();
                        }

                        przyklad.setXY(przyklad.getXY().x, przyklad.getXY().y);
                        przyklad.repaint();

                        stolik.repaint();
                        setWyniczek(5);
                        zlyruch = true;
                        break;
                    }
                    if (przyklad.contains(stop) && !wymiana1.pusty() && pochodzenie.contains(start)
                            && wwa4(wymiana1.wwa5(), przyklad.wwa5()))
                    {
                        System.out.println("Dobrze idzie");
                        while (!wymiana1.pusty())
                        {
                            System.out.println("lecisz dalej: " + wymiana1.wwa5().getValue());
                            przyklad.pierwszy(wymiana1.popFirst());
                            pochodzenie.popFirst();
                        }
                        if (pochodzenie.wwa5() != null)
                        {
                            com.company.karta temp = pochodzenie.wwa5().pokazgora();
                            temp.repaint();
                            pochodzenie.repaint();
                        }

                        przyklad.setXY(przyklad.getXY().x, przyklad.getXY().y);
                        przyklad.repaint();

                        stolik.repaint();
                        setWyniczek(5);
                        zlyruch = true;
                        break;
                    }
                }
                for (int x = 0; x < koncowyzestaw; x++)
                {
                    przyklad = koncowekarty[x];

                    if (karta.wwa3() == true && pochodzenie != null && przyklad.contains(stop) && pochodzenie != przyklad)
                    {
                        if (przyklad.pusty())
                        {
                            if (karta.getValue() == com.company.karta.wartosc.as)
                            {
                                com.company.karta c = pochodzenie.popFirst();
                                c.repaint();
                                if (pochodzenie.wwa5() != null)
                                {

                                    com.company.karta temp = pochodzenie.wwa5().pokazgora();
                                    temp.repaint();
                                    pochodzenie.repaint();
                                }

                                przyklad.setXY(przyklad.getXY().x, przyklad.getXY().y);
                                przyklad.pchanie(c);

                                przyklad.repaint();

                                stolik.repaint();

                                System.out.print("pokazanie ");
                                przyklad.pokarozmiar();
                                karta = null;
                                setWyniczek(10);
                                zlyruch = true;
                                break;
                            }
                        } else if (ostetecznyruch(karta, przyklad.getLast()))
                        {
                            com.company.karta c = pochodzenie.popFirst();
                            c.repaint();
                            if (pochodzenie.wwa5() != null)
                            {

                                com.company.karta temp = pochodzenie.wwa5().pokazgora();
                                temp.repaint();
                                pochodzenie.repaint();
                            }

                            przyklad.setXY(przyklad.getXY().x, przyklad.getXY().y);
                            przyklad.pchanie(c);

                            przyklad.repaint();

                            stolik.repaint();

                            System.out.print("pokazanie ");
                            przyklad.pokarozmiar();
                            karta = null;
                            sprawdzeniewygranej = true;
                            setWyniczek(10);
                            zlyruch = true;
                            break;
                        }
                    }

                }
            }
            if (!zlyruch && przyklad != null && karta != null)
            {
                statystyki.setText("To jest niedozwolony ruch");
            }
            if (sprawdzeniewygranej)
            {
                boolean gameNotOver = false;
                for (int x = 0; x < koncowyzestaw; x++)
                {
                    przyklad = koncowekarty[x];
                    if (przyklad.pokarozmiar() != 13)
                    {
                        gameNotOver = true;
                        break;
                    }
                }
                if (!gameNotOver)
                    koniec = true;
            }

            if (sprawdzeniewygranej && koniec)
            {
                JOptionPane.showMessageDialog(stolik, "Gratulacje ! Gra została wygrana!");
                statystyki.setText("KONIEC!");
            }
            start = null;
            stop = null;
            pochodzenie = null;
            przyklad = null;
            karta = null;
            ostetecznyzestaw = false;
            sprawdzeniewygranej = false;
            koniec = false;
        }
    }

    private static void nowagra() {
        zestaw = new karciochy(true);
        zestaw.tasacja();
        stolik.removeAll();

        if (kartydogry != null && koncowekarty != null) {
            for (int x = 0; x < liczbazestawow; x++) {
                kartydogry[x].makeEmpty();
            }
            for (int x = 0; x < koncowyzestaw; x++) {
                koncowekarty[x].makeEmpty();
            }
        }
        koncowekarty = new osattki[koncowyzestaw];
        for (int x = 0; x < koncowyzestaw; x++) {
            koncowekarty[x] = new osattki();

            koncowekarty[x].setXY((ostatecznepolozenie.x + (x * karta.szerokosckarty)) + 10, ostatecznepolozenie.y);
            stolik.add(koncowekarty[x]);

        }

        stolik.add(ruchkarta(nowynawciskkarty, polozeniezestawu.x, polozeniezestawu.y));
        kartydogry = new karciochy[liczbazestawow];
        for (int x = 0; x < liczbazestawow; x++) {
            kartydogry[x] = new karciochy(false);
            kartydogry[x].setXY((polozeniezestawu.x + (x * (karta.szerokosckarty + 10))), polozeniestart.y);

            stolik.add(kartydogry[x]);
        }


        for (int x = 0; x < liczbazestawow; x++) {
            int hld = 0;
            karta c = zestaw.pop().pokazgora();
            kartydogry[x].pierwszy(c);

            for (int y = x + 1; y < liczbazestawow; y++) {
                kartydogry[y].pierwszy(c = zestaw.pop());
            }
        }
        czas = 0;

        nowynawcisk.addActionListener(new NewGameListener());
        nowynawcisk.setBounds(0, wysokoscstolu - 100, 120, 30);

        nawciskkonca.addActionListener(new siema());
        nawciskkonca.setBounds(0, wysokoscstolu - 70, 120, 30);


        przyciskzasad.addActionListener(new ShowRulesListener());
        przyciskzasad.setBounds(120, wysokoscstolu - 70, 120, 30);

        tytulgry.setText("<b>Pasjans</b>");
        tytulgry.setEditable(false);
        tytulgry.setOpaque(false);
        tytulgry.setBounds(245, 20, 100, 100);

        wynik1.setBounds(240, wysokoscstolu - 70, 120, 30);
        wynik1.setText("Wynik: 0");
        wynik1.setEditable(false);
        wynik1.setOpaque(false);

        czas1.setBounds(360, wysokoscstolu - 70, 120, 30);
        czas1.setText("Czas: 0");
        czas1.setEditable(false);
        czas1.setOpaque(false);

        startTimer();

        nawciskczasu.setBounds(480, wysokoscstolu - 70, 125, 30);
        nawciskczasu.addActionListener(new ToggleTimerListener());

        statystyki.setBounds(605, wysokoscstolu - 70, 180, 30);
        statystyki.setEditable(false);
        statystyki.setOpaque(false);

        stolik.add(statystyki);
        stolik.add(nawciskczasu);
        stolik.add(tytulgry);
        stolik.add(czas1);
        stolik.add(nowynawcisk);
        stolik.add(przyciskzasad);
        stolik.add(wynik1);
        stolik.add(nawciskkonca);
        stolik.repaint();
    }
    private static class wyjscie implements ActionListener {
        public void actionPerformed(ActionEvent event) {
            System.exit(0);
        }
    }
    public static void main(String[] args)
        {

            Container contentPane;

        ramka.setSize(szerokoscstolu, wysokoscstolu);

        stolik.setLayout(null);
        stolik.setBackground(new Color(10, 80, 100));

        contentPane = ramka.getContentPane();
        contentPane.add(stolik);
        ramka.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        nowagra();

        stolik.addMouseListener(new CardMovementManager());
        stolik.addMouseMotionListener(new CardMovementManager());

        ramka.setVisible(true);

    }
}
